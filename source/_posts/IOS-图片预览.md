---
title: "IOS 图片预览"
date: 2018-09-06 22:33:44
categories:
	- IOS
	- Swift
tags:
	- GestureRecognizer
	- UIScrollView
description: ""
copyright: true
---

### 介绍

图片浏览器, 实现了单张图片浏览, 支持双击放大缩小, 手势捏合   
不存在将黑边(图片未铺满屏幕的区域, 不能放大查看)

### 完整代码

```
//
//  ImgPreViewController.swift
//
//  Created by syc on 2018/10/18.
//  Copyright © 2018年 623971951. All rights reserved.
//

import UIKit
import Foundation
import AVFoundation

// https://github.com/Krisiacik/ImageViewer
class ImgPreViewController: UIViewController {

    var image: UIImage?
    var imageView: UIImageView!
    var scrollView: UIScrollView!
    var activityIndicatorView: UIActivityIndicatorView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        scrollView = UIScrollView()
        scrollView.showsHorizontalScrollIndicator = false
        scrollView.showsVerticalScrollIndicator = false
        scrollView.decelerationRate = UIScrollViewDecelerationRateFast
        scrollView.contentInset = UIEdgeInsets.zero
        scrollView.contentOffset = CGPoint.zero
        scrollView.minimumZoomScale = 1
        scrollView.maximumZoomScale = 3
        
        scrollView.delegate = self
        
        scrollView.addObserver(self, forKeyPath: "contentOffset", options: NSKeyValueObservingOptions.new, context: nil)

        
        imageView = UIImageView(frame: self.view.bounds)
        imageView.contentMode = .scaleAspectFit
        imageView.image = image
        
        self.view.addSubview(scrollView)
        scrollView.addSubview(imageView)
        
        activityIndicatorView = UIActivityIndicatorView(activityIndicatorStyle: .white)
        activityIndicatorView.startAnimating()
        activityIndicatorView.stopAnimating()
        view.addSubview(activityIndicatorView)
        
        let single = UITapGestureRecognizer(target: self, action: #selector(self.singleAction(sender:)))
        let double = UITapGestureRecognizer(target: self, action: #selector(self.doubleAction(sender:)))
        single.numberOfTapsRequired = 1
        double.numberOfTapsRequired = 2
        single.require(toFail: double)
        scrollView.addGestureRecognizer(single)
        scrollView.addGestureRecognizer(double)
    }
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        
        scrollView.frame = self.view.bounds
        activityIndicatorView.center = view.boundsCenter
        
        if let size = imageView.image?.size , size != CGSize.zero {
            
            let aspectFitItemSize = getAspectFitSize(forContentOfSize: size, inBounds: self.scrollView.bounds.size)
            
            scrollView.contentSize = aspectFitItemSize
            imageView.bounds.size = aspectFitItemSize
            imageView.center = scrollView.boundsCenter
        }
    }
    // Reports the continuous progress of Swipe To Dismiss to the Gallery View Controller
    override open func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey: Any]?, context: UnsafeMutableRawPointer?) {
        
        guard keyPath == "contentOffset" else { return }
        
    }
    deinit {
        self.scrollView.removeObserver(self, forKeyPath: "contentOffset")
    }
    
    @objc func pinAction(sender: UIPinchGestureRecognizer){
        // 捏合手势
        let pin = UIPinchGestureRecognizer(target: self, action: #selector(self.pinAction(sender:)))
        self.imageView.isUserInteractionEnabled = true
        self.imageView.addGestureRecognizer(pin)
        
        let factor:CGFloat = sender.scale
        sender.view?.transform = CGAffineTransform(scaleX: factor, y: factor)
    }
    @objc func doubleAction(sender: UITapGestureRecognizer){
        let touchPoint = sender.location(ofTouch: 0, in: imageView)
        let aspectFillScale = getAspectFillZoomScale(forBoundingSize: scrollView.bounds.size, contentSize: imageView.bounds.size)
        
        if (scrollView.zoomScale == 1.0 || scrollView.zoomScale > aspectFillScale) {
            
            let zoomRectangle = zoomRect(ForScrollView: scrollView, scale: aspectFillScale, center: touchPoint)
            
            UIView.animate(withDuration: 0.5, animations: { [weak self] in
                
                self?.scrollView.zoom(to: zoomRectangle, animated: false)
            })
        }
        else  {
            UIView.animate(withDuration: 0.5, animations: {  [weak self] in
                
                self?.scrollView.setZoomScale(1.0, animated: false)
            })
        }
    }
    @objc func singleAction(sender: Any){
        
    }

}
extension ImgPreViewController: UIScrollViewDelegate{
    func viewForZooming(in scrollView: UIScrollView) -> UIView? {
        return imageView
    }
    func scrollViewDidZoom(_ scrollView: UIScrollView) {
        imageView.center = getContentCenter(forBoundingSize: scrollView.bounds.size, contentSize: scrollView.contentSize)
    }
}

extension ImgPreViewController{
    // https://github.com/Krisiacik/ImageViewer
    func getAspectFillZoomScale(forBoundingSize boundingSize: CGSize, contentSize: CGSize) -> CGFloat {
        
        let aspectFitSize = aspectFitContentSize(forBoundingSize: boundingSize, contentSize: contentSize)
        return (floor(boundingSize.width) == floor(aspectFitSize.width)) ? (boundingSize.height / aspectFitSize.height): (boundingSize.width / aspectFitSize.width)
    }
    func getAspectFitSize(forContentOfSize contentSize: CGSize, inBounds bounds: CGSize) -> CGSize {
        return AVMakeRect(aspectRatio: contentSize, insideRect: CGRect(origin: CGPoint.zero, size: bounds)).size
    }
    func getContentCenter(forBoundingSize boundingSize: CGSize, contentSize: CGSize) -> CGPoint {
        
        let horizontalOffset = (boundingSize.width > contentSize.width) ? ((boundingSize.width - contentSize.width) * 0.5): 0.0
        let verticalOffset   = (boundingSize.height > contentSize.height) ? ((boundingSize.height - contentSize.height) * 0.5): 0.0
        
        return CGPoint(x: contentSize.width * 0.5 + horizontalOffset, y: contentSize.height * 0.5 + verticalOffset)
    }
}
```

### 鸣谢
+ https://github.com/Krisiacik/ImageViewer
