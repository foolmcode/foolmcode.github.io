---
title:  "UICollectionView和UITapGestureRecognizer的tap冲突"
---
UICollectionView的didSelectItemAtIndexPath代理方法和UITapGestureRecognizer的tap冲突
[参考Stack OverFlow](http://stackoverflow.com/a/8198502/6531133)得以解决。

``` 
////////////////////////////////////////////////////////////
// UIGestureRecognizerDelegate methods 
#pragma mark UIGestureRecognizerDelegate methods  

- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldReceiveTouch:(UITouch *)touch
{
    if ([touch.view isDescendantOfView:autocompleteTableView]) {

        // Don't let selections of auto-complete entries fire the 
        // gesture recognizer
        return NO;
    }

    return YES;
}
```

注意需要设置UIGestureRecognizerDelegate的代理。 

```
 UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc]initWithTarget:self action:@selector(closeView)];
    tap.delegate = self;
    [self addGestureRecognizer:tap];
```

观察上面代码，因为只设置了上面tap的代理。所以代理方法只会对这一个手势生效，而不会影响别的手势。一开始我没能理解的时候还在担心会不会影响别的手势的触发，其实担心是多余的。

   

