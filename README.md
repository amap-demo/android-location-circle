# android-location-circle
定位圈动画

本工程为基于高德地图Android SDK进行封装，实现定位精度圈向外缩放的动画效果。
## 前述 ##
- [高德官网申请Key](http://lbs.amap.com/dev/#/).
- 阅读[参考手册](http://a.amap.com/lbs/static/unzip/Android_Map_Doc/index.html).
- 工程基于Android 定位SDK和3D地图SDK实现

## 效果展示 ##
![Screenshot]( https://github.com/amap-demo/android-location-circle/raw/master/apk/picture.jpg )

## 扫一扫安装 ##
![Screenshot]( https://github.com/amap-demo/android-location-circle/raw/master/apk/1479866100.png )

## 使用方法 ##
###1:配置搭建AndroidSDK工程###
- [Android Studio工程搭建方法](http://lbs.amap.com/api/android-sdk/guide/creat-project/android-studio-creat-project/#add-jars).
- [通过maven库引入SDK方法](http://lbsbbs.amap.com/forum.php?mod=viewthread&tid=18786).

## 核心类/接口 ##
| 类    | 接口  | 说明   |
| -----|:-----:|:-----:|
| AMapLocationClient | startLocation() | 开始定位 | 
| AMapLocation | getAccuracy() | 获取定位精度 | 
| AMap | addCircle(CircleOptions options) | 添加circle表示定位精度圈 |
| Circle | setRadius(double radius) | 设置circle的半径 |

## 核心难点 ##

```
   //加载精度圈动画
   public void Scalecircle(final Circle circle) {
        start = SystemClock.uptimeMillis();
        mTimerTask = new circleTask(circle, 1000);
        mTimer.schedule(mTimerTask, 0, 30);
    }
    //定位精度圈半径变化的定时器
    private  class circleTask extends TimerTask {
        private double r;
        private Circle circle;
        private long duration = 1000;

        public circleTask(Circle circle, long rate){
            this.circle = circle;
            this.r = circle.getRadius();
            if (rate > 0 ) {
                this.duration = rate;
            }
        }
        @Override
        public void run() {
            try {
                long elapsed = SystemClock.uptimeMillis() - start;
                float input = (float)elapsed / duration;
//                外圈放大后消失
                float t = interpolator1.getInterpolation(input);
                double r1 = (t + 1) * r;
                circle.setRadius(r1);
                if (input > 2){
                    start = SystemClock.uptimeMillis();
                }
            } catch (Throwable e) {
               e.printStackTrace();
            }
        }
    }
```
