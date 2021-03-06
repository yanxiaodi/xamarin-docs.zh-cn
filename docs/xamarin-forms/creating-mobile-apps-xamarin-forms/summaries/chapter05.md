---
title: 第 5 章： 的摘要。 处理大小
description: 使用 Xamarin.Forms 创建移动应用： 第 5 章： 的摘要。 处理大小
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0b93942d6623106a5e507d6eef3e7140f9d409bd
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241642"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>第 5 章： 的摘要。 处理大小

到目前为止已遇到 Xamarin.Forms 中的几种大小：

- IOS 状态条的高度为 20
- `BoxView`具有默认宽度和高度 40
- 默认值`Padding`中`Frame`为 20
- 默认值`Spacing`上`StackLayout`为 6
- `Device.GetNamedSize`方法将返回数字的字体大小

这些大小不是像素。 相反，它们是独立识别每个平台的独立于设备的单位。

## <a name="pixels-points-dps-dips-and-dius"></a>像素、 点、 分发点、 Dip 和 DIUs

在 Apple Mac 和 Microsoft Windows 的历史记录，早期程序员合作，以像素为单位。 但是，更高分辨率显示面临需要屏幕坐标的更多虚拟化和抽象方法。 在 Mac 世界中，程序员的工作单位*点*、 传统上 1/72 英寸，而 Windows 开发人员能够使用*设备无关的单位*(DIUs) 基于 1/96 英寸。

移动设备，但是，通常保留更接近面临的并且具有较高分辨率比桌面意味着可以容忍较高的像素密度的屏幕。

面向 Apple iPhone 和 iPad 设备的程序员继续工作的单位*点*，但有 160 这些指向英寸。 根据设备，可能有 1、 2 或 3 个像素到点。

Android 是类似的。 程序员工作的单位*密度无关的像素*(dp) 和 dp 以及像素之间的关系基于为一英寸 160 dp。

Windows 运行时还建立了表示接近 160 独立于设备的单位为一英寸的内容的缩放系数。

总之，面向手机和平板电脑 Xamarin.Forms 程序员可以假定所有计价单位都基于以下条件：

- 160 单位为一英寸，等效于
- 64 单位为厘米

只读[ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/)和[ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/)属性定义`VisualElement`具有"模拟"的值的默认&ndash;1。 仅在大小并在布局中容纳了元素时，才将这些属性可以反映设备无关的单位中的元素的实际大小。 此大小包括任何`Padding`的元素上设置但不是`Margin`。

可视元素，将引发[ `SizeChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/)事件时其`Width`或`Height`已更改。 [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize)示例使用此事件来显示的程序的屏幕大小。

## <a name="metrical-sizes"></a>度量大小

[ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView)使用[ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)和[ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/)以显示`BoxView`一英寸高，另一个厘米宽。

## <a name="estimated-font-sizes"></a>估计的字体大小

[**字体**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes)示例演示如何使用 160 单位到英寸规则以点为单位指定字体大小。 在使用这种技术平台的 visual 一致性优于`Device.GetNamedSize`。

## <a name="fitting-text-to-available-size"></a>根据调整文本可用大小

可以通过计算适合的特定矩形范围内的文本块`FontSize`的`Label`使用以下条件：

- 行距是 120%的字体大小 （在 Windows 平台上的 130 %）。
- 平均字符宽度是字体大小的 50%。

[ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize)示例演示这种方法。 此程序已写入之前[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)属性已可用，因此它使用[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)与[ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)设置以模拟边距。

[![估计的字体大小的三个屏幕截图](images/ch05fg07-small.png "文本适应可用大小")](images/ch05fg07-large.png#lightbox "文本适应可用大小")

## <a name="a-fit-to-size-clock"></a>调整大小时钟

[ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock)示例演示如何使用[ `Device.StartTimer` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.StartTimer/p/System.TimeSpan/System.Func%7BSystem.Boolean%7D/)启动定期通知应用程序时时间来更新时钟的计时器。 字体大小设置为一个第六个的页宽，以使显示尽可能大。

## <a name="accessibility-issues"></a>可访问性问题

**EstimatedFontSize**程序和**FitToSizeClock**程序这两个包含存在细微的缺陷： 如果用户更改 Android 或 Windows 10 移动版，程序不再上的电话的辅助功能设置可以估计多大呈现文本的字号基于。 [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest)示例演示此问题。

## <a name="empirically-fitting-text"></a>根据经验调整文本

另一种方法适合于由矩形的文本是根据经验计算呈现的文本大小并对其进行调整，向上或向下。 簿调用中的程序[ `GetSizeRequest` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/)上以获取元素的所需的大小的可视元素。 方法已被否决，并应改为调用程序[ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)。

有关`Label`，第一个参数应是 （要允许包装） 的容器的宽度，同时第二个参数应设置到`Double.PositiveInfinity`以使不受约束的高度。 [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize)示例演示这种方法。



## <a name="related-links"></a>相关链接

- [第 5 章： 完整文本 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [第 5 章： 示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [第 5 章： F # 示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
