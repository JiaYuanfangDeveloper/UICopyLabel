# UICopyLabel
实现复制粘贴剪切功能的UICopyLabel
导入文件到项目中，导入UICopyLabel.h头文件，就可以用UICopyLabel实例化就能用了

01.创建继承自UILabel的 UICopyLabel。
02.当前使用的是系统自带的复制功能。如果要实现自定义按钮功能，打开注释掉的代码就可以了。如果项目有需求，也可以自定义实现其他需要的功能。

```
+++++++++++++++++++++++++  UICopyLabel.h文件   ++++++++++++++++++++++++++++++++
//
//  UICopyLabel.h
//  JExt
//
//  Created by Mr_Jia on 2017/7/24.
//  Copyright © 2017年 Mr_Jia. All rights reserved.
//

#import <UIKit/UIKit.h>

@interface UICopyLabel : UILabel

@end


+++++++++++++++++++++++++  UICopyLabel.m文件   ++++++++++++++++++++++++++++++++
//
//  UICopyLabel.m
//  JExt
//
//  Created by Mr_Jia on 2017/7/24.
//  Copyright © 2017年 Mr_Jia. All rights reserved.
//

#import "UICopyLabel.h"

@implementation UICopyLabel

/*
// Only override drawRect: if you perform custom drawing.
// An empty implementation adversely affects performance during animation.
- (void)drawRect:(CGRect)rect {
    // Drawing code
}
*/

- (instancetype)initWithFrame:(CGRect)frame {
    self = [super initWithFrame:frame];
    
    if (self) {
        [self pressAction];
    }
    return self;
    
}
- (void)pressAction {
    self.userInteractionEnabled = YES;
    
    UILongPressGestureRecognizer *longPress = [[UILongPressGestureRecognizer alloc]initWithTarget:self action:@selector(longPressAction:)];
    longPress.minimumPressDuration = 1;
    [self addGestureRecognizer:longPress];
}

- (BOOL)canBecomeFirstResponder {
    return YES;
}

- (BOOL)canPerformAction:(SEL)action withSender:(id)sender {
    //自定义实现复制粘贴剪切功能
//    if ((action == @selector(customCopy:) && self.text)
//        || (action == @selector(customCut:) && self.text)
//        || action == @selector(customPaste:)) {
//        
//        return YES;
//    }
    if ((action == @selector(cut:) && self.text)
     || (action == @selector(copy:) && self.text)
     || action == @selector(paste:)) {
        
        return YES;
    }
         return NO;
}
- (void)cut:(UIMenuController *)menu
 {
     UIPasteboard *pasteBoard = [UIPasteboard generalPasteboard];
     pasteBoard.string = self.text;
     self.text = nil;
     
    }

- (void)copy:(UIMenuController *)menu
 {
     UIPasteboard *pasteBoard = [UIPasteboard generalPasteboard];
     pasteBoard.string = self.text;
     
 }

 - (void)paste:(UIMenuController *)menu
 {
     UIPasteboard *pasteBoard = [UIPasteboard generalPasteboard];
     self.text = pasteBoard.string;
     }
- (void)customCopy:(id)sender {
    UIPasteboard *pasteBoard = [UIPasteboard generalPasteboard];
    pasteBoard.string = self.text;
}
- (void)customPaste:(id)sender {
    UIPasteboard *pasteBoard = [UIPasteboard generalPasteboard];
    self.text = pasteBoard.string;
}
- (void)customCut:(id)sender {
    UIPasteboard *pasteBoard = [UIPasteboard generalPasteboard];
    pasteBoard.string = self.text;
    self.text = nil;
}
- (void)longPressAction:(UIGestureRecognizer *)recognizer {
    [self becomeFirstResponder];
    
//    UIMenuItem *copyItem = [[UIMenuItem alloc]initWithTitle:@"复制" action:@selector(customCopy:)];
//    UIMenuItem *cutItem = [[UIMenuItem alloc]initWithTitle:@"剪切" action:@selector(customCut:)];
//    UIMenuItem *pasteItem = [[UIMenuItem alloc]initWithTitle:@"粘贴" action:@selector(customPaste:)];
//
//    
//    [[UIMenuController sharedMenuController] setMenuItems:@[copyItem,cutItem,pasteItem]];
    
    [[UIMenuController sharedMenuController] setTargetRect:self.frame inView:self.superview];

    [[UIMenuController sharedMenuController] setMenuVisible:YES animated:YES];
}
@end
```
