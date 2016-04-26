---
title: android异步更新UI的几种方法
date: 2016-04-26 13:42:33
tags:
---
# android异步更新UI的几种方法

----

## 前言

 我们知道在Android开发中不能在非ui线程中更新ui，但是，有的时候我们需要在代码中执行一些诸如访问网络、查询数据库等耗时操作，为了不阻塞ui线程，我们时常会开启一个新的线程（工作线程）来执行这些耗时操作，然后我们可能需要将查询到的数据渲染到ui组件上，那么这个时候我们就需要考虑异步更新ui的问题了。

### android中有下列几种异步更新ui的解决办法：

+ Activity.runOnUiThread(Runnable)
+ View.post(Runnable)
+ long) View.postDelayed(Runnable, long)
+ 使用handler（线程间通讯）（推荐）
+ AsyncTask（推荐）

------
对于下面这段代码:

        public void onClick(View v) {
            new Thread(new Runnable() {
                public void run() {
                   Bitmap bitmap = loadImageFromNetwork("http://example.com/image.png");
                   mImageView.setImageBitmap(bitmap);     
                }
            }).start();
        }


这段代码是一个按钮点击事件的响应方法，当点击了这个按钮后开启了一个子线程去网络上加载图片，然后在这个线程中给imageView设置了图片（更新了ui），这段代码在非ui线程中更新了ui，运行会引发错误。


## 1. Activity.runOnUiThread：


通常，在Activity，我们可以使用Activity的runOnUiThread方法来更新ui。

如：

          public void onClick(View v) {
          new Thread(new Runnable() {
            public void run() {
               Bitmap bitmap = loadImageFromNetwork("http://example.com/image.png");
               runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        mImageView.setImageBitmap(bitmap);  
                    }
               });              
            }
          }).start();
          }

## 2. View.post(Runable)


View类及其子类提供了一个post(Runable)方法允许我们将我们要做的操作放到这个匿名Runable对象的run方法中，在这个方法里面我们可以直接更新ui。

如：

      public void onClick(View v) {
      new Thread(new Runnable() {
        public void run() {
           Bitmap bitmap = loadImageFromNetwork("http://example.com/image.png");
           imageView.post(new Runnable() {
              @Override
              public void run() {
                  mImageView.setImageBitmap(bitmap);  
              }
           });             
        }
      }).start();
      }

## 3. View.postDelayed(Runnable, long)

和View.post(Runable)方法一样，只是延迟第二个参数指定的时间后执行，而View.post(Runable)是立即执行。

        public void onClick(View v) {
        new Thread(new Runnable() {
          public void run() {
             Bitmap bitmap = loadImageFromNetwork("http://example.com/image.png");  
             imageView.postDelayed(new Runnable() {
                @Override
                public void run() {
                    mImageView.setImageBitmap(bitmap);  
                }
             },2000);          
          }
        }).start();
        }


## 4. 使用Handler（推荐）

前面说道的几种方法当这种操作过多的时候，我们的代码会显得臃肿，代码及业务都难于管理控制，所以，当这类代码多的时候我们就应该采取Handler的方式了。

如：

子线程

          new Thread(new Runnable() {
          public void run() {
            Bitmap bitmap = loadImageFromNetwork("http://example.com/image.png");  
            Message message = mHandler.obtainMessage();
            message.what = 1;
            message.obj = bitmap;
            mHandler.sendMessage(message);        
          }
          }).start();

主线程

          Handler mHandler = new Handler(){
              @Override
              public void handleMessage(Message msg) {
                  switch (msg.what){
                      case 1:
                          Bitmap bitmap = (Bitmap) msg.obj;
                          imageView.setImageBitmap(bitmap);
                          break;
                      case 2:
                          // ...
                          break;
                      default:
                          break;
                  }
              }
          };




## 5. AsyncTask（推荐）

android为我们提供了异步任务AsyncTask，我们可以使用AsyncTask轻松地实现异步加载数据及更新ui。

如：

            AsyncTask<String,Void,Bitmap> asyncTask = new AsyncTask<String, Void, Bitmap>() {

            /**
             * 即将要执行耗时任务时回调，这里可以做一些初始化操作
             */
            @Override
            protected void onPreExecute() {
                super.onPreExecute();
            }

            /**
             * 在后台执行耗时操作，其返回值将作为onPostExecute方法的参数
             * @param params
             * @return
             */
            @Override
            protected Bitmap doInBackground(String... params) {
                Bitmap bitmap = loadImageFromNetwork(params[0]);
                return bitmap;
            }

            /**
             * 当这个异步任务执行完成后，也就是doInBackground（）方法完成后，
             * 其方法的返回结果就是这里的参数
             * @param bitmap
             */
            @Override
            protected void onPostExecute(Bitmap bitmap) {
                imageView.setImageBitmap(bitmap);
            }
            };
            asyncTask.execute("http://example.com/image.png");


需要知道的是doInBackground方法工作在工作线程中，所以，我们在这个方法里面执行耗时操作。同时，由于其返回结果会传递到onPostExecute方法中，而onPostExecute方法工作在UI线程，这样我们就在这个方法里面更新ui，达到了异步更新ui的目的。

事实上，对于android的异步加载数据及更新ui，我们不仅可以选择AsyncTask异步任务，还可以选择许多开源的网络框架，如xUtils，Volley，Okhttp，…，这些优秀的网络框架让我们异步更新ui变得非常简单，而且，效率和性能也非常高。

  ## 总结：

对于上面的许多解决办法，其实它们做的都是同一件事情，即在工作线程中执行耗时任务，然后在ui线程中更新ui，只不过过程不一样，有得直接给我们封装好了，有得需要我们自己控制管理。
