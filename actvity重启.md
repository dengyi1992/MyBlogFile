---
title: actvity重启
date: 2016-05-04 17:54:07
tags:
---
###  重启活动
---
代码如下

    public void reload() {
       Intent intent = getIntent();
       overridePendingTransition(0, 0);
       intent.addFlags(Intent.FLAG_ACTIVITY_NO_ANIMATION);
       finish();
       overridePendingTransition(0, 0);
       startActivity(intent);
    }
---
测试如下：

    package com.example.deng.restartactivity;

    import android.content.Intent;
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import android.view.View;
    import android.widget.Button;

    public class MainActivity extends AppCompatActivity {

        private Button mRestartButton;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            mRestartButton = (Button) findViewById(R.id.restart);

            mRestartButton.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    reload();
                }
            });
            System.out.println("onCreate");
        }
        public void reload() {
            Intent intent = getIntent();
            overridePendingTransition(0, 0);
            intent.addFlags(Intent.FLAG_ACTIVITY_NO_ANIMATION);
            finish();
            overridePendingTransition(0, 0);
            startActivity(intent);
        }

        @Override
        protected void onDestroy() {
            super.onDestroy();
            System.out.println("onDestroy");
        }
    }

<img src="/actvity重启/layout-2016-05-04-180456.png">


#### 结果如下

      05-04 10:00:14.729 17465-17465/com.example.deng.restartactivity I/System.out: onCreate
      05-04 10:00:14.814 17465-17477/com.example.deng.restartactivity I/art: Background partial concurrent mark sweep GC freed 10316(454KB) AllocSpace objects, 2(45KB) LOS objects, 43% free, 1309KB/2MB, paused 269us total 198.677ms
      05-04 10:00:14.931 17465-17494/com.example.deng.restartactivity W/EGL_emulation: eglSurfaceAttrib not implemented
      05-04 10:00:14.931 17465-17494/com.example.deng.restartactivity W/OpenGLRenderer: Failed to set EGL_SWAP_BEHAVIOR on surface 0xa4cfb2c0, error=EGL_SUCCESS
      05-04 10:00:15.119 17465-17465/com.example.deng.restartactivity I/System.out: onDestroy
      05-04 10:00:27.328 17465-17465/com.example.deng.restartactivity I/System.out: onCreate
      05-04 10:00:27.440 17465-17494/com.example.deng.restartactivity W/EGL_emulation: eglSurfaceAttrib not implemented
      05-04 10:00:27.440 17465-17494/com.example.deng.restartactivity W/OpenGLRenderer: Failed to set EGL_SWAP_BEHAVIOR on surface 0xa4cfb260, error=EGL_SUCCESS
      05-04 10:00:27.720 17465-17465/com.example.deng.restartactivity I/System.out: onDestroy
      05-04 10:00:28.416 17465-17465/com.example.deng.restartactivity I/System.out: onCreate
      05-04 10:00:28.582 17465-17494/com.example.deng.restartactivity W/EGL_emulation: eglSurfaceAttrib not implemented
      05-04 10:00:28.582 17465-17494/com.example.deng.restartactivity W/OpenGLRenderer: Failed to set EGL_SWAP_BEHAVIOR on surface 0xa4cfb8e0, error=EGL_SUCCESS
      05-04 10:00:28.792 17465-17465/com.example.deng.restartactivity I/System.out: onDestroy

每一次点击按钮的时候都是先启动然后再销毁前一个
