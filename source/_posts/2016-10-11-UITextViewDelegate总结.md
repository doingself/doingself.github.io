---
title: UITextViewDelegate总结
date: 2016-10-11 22:33:44
categories:
    - IOS
    - Swift
tags:
	- UITextViewDelegate
description: "UITextViewDelegate总结"
copyright: true
---

```
extension ViewController: UITextViewDelegate{
    /*
     几乎所有操作都会触发textViewDidChangeSelection，包括点击文本框、增加内容删除内容
     执行率高于 textViewDidBeginEditing 代理 也高于 UIKeyboardWillShowNotification 通知
     */
    func textViewDidChangeSelection(_ textView: UITextView) {
        print("\t textViewDidChangeSelection 几乎所有操作都会触发")
    }
    ///是否可以开始编辑
    func textViewShouldBeginEditing(_ textView: UITextView) -> Bool {
        print("\t textViewShouldBeginEditing 是否可以开始编辑")
        return true
    }
    ///是否可以结束编辑
    func textViewShouldEndEditing(_ textView: UITextView) -> Bool {
        print("\t textViewShouldEndEditing 是否可以结束编辑")
        return true
    }
    /// 开始编辑
    func textViewDidBeginEditing(_ textView: UITextView) {
        print("\t textViewDidBeginEditing 开始编辑")
    }
    /// 结束编辑
    func textViewDidEndEditing(_ textView: UITextView) {
        print("\t textViewDidEndEditing 结束编辑")
    }
    /// 编辑过程中触发
    /// 只有在内容改变时才触发，而且这个改变内容是手动输入有效
    func textViewDidChange(_ textView: UITextView) {
        print("\t textViewDidChange 编辑过程中触发")
    }
    /// 编辑过程中触发，是否将 变更的内容 写入textView
    func textView(_ textView: UITextView, shouldChangeTextIn range: NSRange, replacementText text: String) -> Bool {
        print("\t 变更范围 \(range.length) \(range.location) - 变更内容 \(text)")
        return true
    }
    
    func textView(_ textView: UITextView, shouldInteractWith URL: URL, in characterRange: NSRange) -> Bool {
        print("\t shouldInteractWith \(URL)")
        return true
    }
    func textView(_ textView: UITextView, shouldInteractWith textAttachment: NSTextAttachment, in characterRange: NSRange) -> Bool {
        print("\t shouldInteractWith \(textAttachment)")
        return true
    }
    
    @available(iOS 10.0, *)
    func textView(_ textView: UITextView, shouldInteractWith URL: URL, in characterRange: NSRange, interaction: UITextItemInteraction) -> Bool {
        print("\t shouldInteractWith \(URL)")
        return true
    }
    @available(iOS 10.0, *)
    func textView(_ textView: UITextView, shouldInteractWith textAttachment: NSTextAttachment, in characterRange: NSRange, interaction: UITextItemInteraction) -> Bool {
        print("\t shouldInteractWith \(textAttachment)")
        return true
    }
}
```
