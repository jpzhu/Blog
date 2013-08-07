# 在android应用中打印当前的调用栈信息

    StackTraceElement[] ste = ts.get(Thread.currentThread());
    for (StackTraceElement s : ste) {
        Log.i(TAG, " " + s.toString());//s is stack information you want print.
    }
