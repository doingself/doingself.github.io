
用于将 接口请求数据 转换为对象

```
struct SYCUser: Codable{
    let id: Int
    let userName: String
    let avatar: String
    let accessToken: String
    
    private enum CodingKeys: String, CodingKey {
        case id
        case userName = "user_name"
        case avatar
        case accessToken = "access_token"
    }
}
```

接口定义
```

import Foundation
import Moya

/// 接口
enum SYCEndPoint {
    /// 登录
    case login(String)
    /// 获取 socket 配置
    case socket
    /// 注销
    case logout(String)
    /// 上传文件
    case upload(fileUrl: URL, fileName: String)
    case uploadMultipart(fileUrls: [URL], uploadType: String, fileCount: Int)
    /// 下载
    case download(filePath: String)
    case downloadSaveName(filePath: String, saveName: String)
}

extension SYCEndPoint: TargetType{
    /// 服务器地址
    var baseURL: URL {
        return URL(string: "base")!
    }
    
    
    /// 接口地址
    var path: String {
        switch self {
        case .login:
            return "login"
        case .logout:
            return "logout"
        case .socket:
            return "socket"
        case let .upload(_, fileName):
            return "upload" + fileName
        case .uploadMultipart:
            return "upload multipart"
        default:
            return "undefine"
        }
    }
    
    /// 请求方式
    var method: Moya.Method {
        switch self {
        case .login, .logout:
            return .post
            
        default:
            return .get
        }
    }
    
    /// 测试数据
    var sampleData: Data {
        return "".data(using: String.Encoding.utf8)!
    }
    
    /// 发起请求
    var task: Task {
        switch self {
        case .login(let user):
            var params = [String: Any]()
            params["user"] = user
            return .requestParameters(parameters: params, encoding: URLEncoding.default)
            
        case .logout(let user):
            var params = [String: Any]()
            params["user"] = user
            return .requestParameters(parameters: params, encoding: URLEncoding.default)
            
        case let .upload(fileUrl, _):
            return .uploadFile(fileUrl)
            
        case let .uploadMultipart(fileUrls, uploadType, fileCount):
            
            // url 参数
            let urlParam = ["param1": uploadType]
            
            // request body 参数
            let intData = String(fileCount).data(using: .utf8)!
            let intFormat = MultipartFormData(provider: .data(intData), name: "param2")
            
            let firstFile = MultipartFormData(provider: MultipartFormData.FormDataProvider.file(fileUrls.first!), name: "file1", fileName: "firstname.png", mimeType: "image/png")
            
            let lastFile = MultipartFormData(provider: .file(fileUrls.last!), name: "file2", fileName: "lastname.png", mimeType: "image/png")
            
            //.uploadMultipart([intFormat, firstFile, lastFile])
            return .uploadCompositeMultipart([intFormat, firstFile, lastFile], urlParameters: urlParam)
            
        case .download:
            return .downloadDestination(defaultDownloadDestination)
            
        case let .downloadSaveName(_, saveName):
            let savePath = defaultDownloadPath.appendingPathComponent(saveName)
            let downLoadDestination: DownloadDestination = { _, _ in
                // 覆盖同名
                return (savePath, [.removePreviousFile])
            }
            return .downloadDestination(downLoadDestination)
            
        default:
            return .requestPlain
        }
    }
    
    var headers: [String : String]? {
        return nil
    }
    
    
}

let defaultDownloadPath: URL = {
    let documents = FileManager.default.urls(for: FileManager.SearchPathDirectory.documentDirectory, in: FileManager.SearchPathDomainMask.userDomainMask)
    if #available(iOS 10.0, *) {
        return documents.first ?? FileManager.default.temporaryDirectory
    } else {
        // Fallback on earlier versions
        return documents.first!
    }
}()

private let defaultDownloadDestination: DownloadDestination = { (temporaryURL: URL,response: HTTPURLResponse) in
    
    // 不改变文件名, 同名不覆盖
    //return (defaultDownloadPath.appendingPathComponent(response.suggestedFilename!), [])
    
    // 不改变文件名, 同名覆盖
    return (defaultDownloadPath.appendingPathComponent(response.suggestedFilename!), [.removePreviousFile])
}

```

接口请求
```

import Foundation
import Moya
import Result

struct SYCProvider{
    static let provider = MoyaProvider<SYCEndPoint>()
    
    /// 网络请求
    ///
    /// - Parameters:
    ///   - endpoint: 接口
    ///   - success: 成功回调
    ///   - error: 错误码
    ///   - failure: 失败回调
    static func request(
        endpoint: SYCEndPoint,
        success: @escaping (Any) -> Void,
        error: @escaping (Int) -> Void,
        failure: @escaping (MoyaError) -> Void) -> Void{
        
        provider.request(endpoint) { (result: Result<Response, MoyaError>) in
            switch result{
            case let .success(response):
                do {
                    // 成功
                    _ = try response.filterSuccessfulStatusCodes()
                    let json = try response.mapJSON()
                    success(json)
                }catch let err {
                    // 失败
                    print("错误原因：\(err.localizedDescription)")
                    let statusCode = (err as! MoyaError).response!.statusCode
                    error(statusCode)
                }
            case let .failure(error):
                failure(error)
            }
        }
    }
    
    static func upload(){
        
    }
    
    
    
    // MARK: test
    /// 下载单个文件
    func testDownload(){
        let filePtah = "xx/xx"
        let endpoint = SYCEndPoint.download(filePath: filePtah)
        SYCProvider.provider.request(endpoint, callbackQueue: nil, progress: { (progress: ProgressResponse) in
            //实时打印出下载进度
            print("当前进度: \(progress.progress)")
        }) { (result: Result<Response, MoyaError>) in
            if case .success = result{
                let file = defaultDownloadPath.appendingPathComponent(filePtah)
                print(file)
            }
        }
    }
    /// 下载文件保存自定义文件名
    func testDo(){
        let fileName = "file"
        let filePath = "file/path"
        let endpoint = SYCEndPoint.downloadSaveName(filePath: filePath, saveName: fileName)
        SYCProvider.provider.request(endpoint, callbackQueue: nil, progress: { (progress: ProgressResponse) in
            //实时打印出下载进度
            print("当前进度: \(progress.progress)")
        }) { (result: Result<Response, MoyaError>) in
            if case .success = result{
                let file = defaultDownloadPath.appendingPathComponent(fileName)
                print(file)
            }
        }
    }
    /// 上传文件
    func testUpload(){
        let file = URL(fileURLWithPath: "file/path")
        let endpoint = SYCEndPoint.upload(fileUrl: file, fileName: "file-name")
        SYCProvider.provider.request(endpoint, callbackQueue: nil, progress: { (progress: ProgressResponse) in
            //实时打印出上传进度
            print("当前进度: \(progress.progress)")
        }) { (result: Result<Response, MoyaError>) in
            if case let .success(response) = result{
                // 响应状态码：200, 401, 500...
                let statusCode = response.statusCode
                print(statusCode)
            }
        }
    }
    
    /// 上传多个文件
    func testUploadMultipart(){
        let firstUrl = URL(string: "xx/xx")!
        let lastUrl = URL(string: "xx/xx")!
        let endpoint = SYCEndPoint.uploadMultipart(fileUrls: [firstUrl, lastUrl], uploadType: "imageHaha", fileCount: 2)
        SYCProvider.provider.request(endpoint, callbackQueue: nil, progress: { (progress: ProgressResponse) in
            //实时打印出上传进度
            print("当前进度: \(progress.progress)")
        }) { (result: Result<Response, MoyaError>) in
            if case let .success(response) = result{
                // 响应状态码：200, 401, 500...
                let statusCode = response.statusCode
                print(statusCode)
            }
        }
    }
    
    /// 使用自己封装的 request
    func testLogin1(){
        
        let loginEndpoint = SYCEndPoint.login("user")
        SYCProvider.request(endpoint: loginEndpoint, success: { (result) in
            // success
        }, error: { (statusCode) in
            // code
        }) { (err: MoyaError) in
            // err
        }
    }
    /// 使用 provider 的 request
    func testLogin2(){
        // 测试登录
        let loginEndpoint = SYCEndPoint.login("user")
        
        SYCProvider.provider.request(loginEndpoint) { (result: Result<Response, MoyaError>) in
            switch result{
            case .success(let response):
                // 请求成功
                // 响应状态码：200, 401, 500...
                //let statusCode = response.statusCode
                // 响应数据
                //let data = response.data
                //let json = try? response.mapJSON()
                //let model = try JSONDecoder().decode(SYCUser.self, from: data)
                
                do {
                    // 成功
                    _ = try response.filterSuccessfulStatusCodes()
                    _ = try response.mapJSON()
                }catch let err {
                    // 失败
                    print("错误原因：\(err.localizedDescription)")
                }
                
            case .failure(let err):
                // 请求失败
                print("错误原因：\(err.errorDescription!)")
                switch err{
                case .imageMapping(let response):
                    print(response)
                case .jsonMapping(let response):
                    print(response)
                case .statusCode(let response):
                    print(response)
                default:
                    break
                }
                break
            }
        }
    }
}
```
