# SystemShare
前言
分享是APP开发必不可少的一个功能。APP分享是产品推广，增加用户量的一个重要渠道，所以分享功能的开发已经是非常成熟了，网上也有很多第三方的分享库，比如，友盟，Mob等。而我今天主要是要讲苹果系统自带的原生分享的实现。

参考文章
iOS 社会化分享方案总结;

1.优劣对比
第三方分享与原生分享，各有优缺点，并没有优劣之分，主要是看APP的需求与你个人的喜好。

①原生分享

优点
使用简单、不用注册繁杂的分享平台账号、不用导入臃肿的SDK包
缺点
UI可定制性差，只能使用系统提供的固定样式。分享的类型受限，只能分享text、url、image。
②第三方分享

优点
开发者可以按照第三方文档自己定制分享界面UI、功能。分享的类型可以自由选择，除了text、url、image。还有其他格式多媒体(声音、视频、文件等)可供选择。
缺点
需要去友盟以及各个分享平台注册繁杂的账号、导入臃肿的SDK包。配置跳转白名单。
2.原生分享实现
说了优缺点，下面再说一下功能的具体实现。第三方分享的实现，网上已经有很多文章，各个平台也有自己的开发文档以及技术支持，我这里就不再赘述，下面主要说一下原生分享的实现。

- (void)shareButtonDidTouched:(UIButton *)button
{
    NSString *textToShare1 = @"要分享的文本内容";
    UIImage *imageToShare = [UIImage imageNamed:@"redEnvelope"];
    NSURL *urlToShare = [NSURL URLWithString:@"http://www.baidu.com"];
    NSArray *activityItems = @[textToShare1, imageToShare, urlToShare];
    UIActivityViewController *activityVC = [[UIActivityViewController alloc]initWithActivityItems:activityItems applicationActivities:nil];

    //去除一些不需要的图标选项
    activityVC.excludedActivityTypes = @[UIActivityTypePostToFacebook, UIActivityTypeAirDrop, UIActivityTypePostToWeibo, UIActivityTypePostToTencentWeibo];

    //成功失败的回调block
    UIActivityViewControllerCompletionWithItemsHandler myBlock = ^(UIActivityType __nullable activityType, BOOL completed, NSArray * __nullable returnedItems, NSError * __nullable activityError) {

        if (completed){
            NSLog(@"completed");
        }else{
            NSLog(@"canceled");
        }
   };
   activityVC.completionWithItemsHandler = myBlock;

    [self presentViewController:activityVC animated:YES completion:nil];
}
值得一提的是苹果原生分享还能判断是否分享成功，而微信原生的分享接口已经不再区分成功失败的回调，而第三方的微信分享其实就是对微信原生分享的一层封装，所以第三方分享也不再区分分享成功失败的返回。
微信分享功能调整公告

如你所见，原生分享，只需要实现👆上面的那一段代码，实现简单的优势十分明显，但如上所述，缺点也十分明显，样式流程固定，如下图


分享流程图一

分享流程图二

分享出去后的样式如下图
原生分享效果图

而第三方的分享，UI定制化程度高，而且分享出去后还会带有APP的图标及名称，可识别程度高，更利于推广。如下图


第三方分享效果图

所以，最终选择哪种分享效果，需要你自己去权衡把握。
3.题外
如上图“分享流程图一”所示，原生分享所能选择的APP是固定，那怎样才能让自己的APP出现在原生的分享列表呢？(持续更新ing)
这里先给个连接"iOS实现App之间的内容分享"。但这里实现的其实是文件的跨APP分享，比如图片，PDF，文档等，而不是像链接，文字等内容的分享。文件的跨APP分享，还是比较好实现，看上连接的图片就行，不再赘述。而内容的分享，暂时还没找到相关的资料，可能用到App Extension，后续再研究研究，有知道的大神也希望留个言，指点指点。
原生分享列表
以上，iOS原生分享实现，以及优缺点描述。如有不妥之处，还望斧正。
