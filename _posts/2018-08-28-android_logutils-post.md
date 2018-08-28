---
title: "Android LogUtils.java"
date: 2018-08-28 16:44:00 -0400
categories: android log
---





import android.util.Log;

import org.json.JSONArray;
import org.json.JSONObject;

import java.io.Serializable;
import java.util.Iterator;

public final class LogUtil implements Serializable {

    private static final boolean LOG_ENABLE = BuildConfig.DEBUG;
    private static final boolean DETAIL_ENABLE = BuildConfig.DEBUG;

    private static String TAG = "LOGGER";

    private static String buildMsg(String msg) {
        StringBuilder buffer = new StringBuilder();

        if(DETAIL_ENABLE) {
            final StackTraceElement ste = Thread.currentThread().getStackTrace()[4];

            buffer.append("[");
            buffer.append(Thread.currentThread().getName());
            buffer.append(": ");
            buffer.append(ste.getFileName());
            buffer.append(": ");
            buffer.append(ste.getLineNumber());
            buffer.append(":");
            buffer.append(ste.getMethodName());
            buffer.append("()] _____ ");
        }

        buffer.append(msg);

        return buffer.toString();
    }

    public static void v(String msg) {
        v(TAG, buildMsg(msg));
    }

    public static void d(String msg) {
        d(TAG, buildMsg(msg));
    }

    public static void i(String msg) {
        i(TAG, buildMsg(msg));
    }

    public static void w(String msg) {
        w(TAG, buildMsg(msg));
    }

    public static void w(String msg, Exception e) {
        w(TAG, buildMsg(msg), e);
    }

    public static void e(String msg) {
        e(TAG, buildMsg(msg));
    }

    public static void e(Exception e) {
        e(TAG, "ERROR", e);
    }

    public static void e(String msg, Exception e) {
        e(TAG, buildMsg(msg), e);
    }

    public static void v(String tag, String msg) {
        if(LOG_ENABLE) {
            Log.v(tag, buildMsg(msg));
        }
    }

    public static void d(String tag, String msg) {
        if(LOG_ENABLE) {
            Log.d(tag, buildMsg(msg));
        }
    }

    public static void i(String tag, String msg) {
        if(LOG_ENABLE) {
            Log.i(tag, buildMsg(msg));
        }
    }

    public static void w(String tag, String msg) {
        if(LOG_ENABLE) {
            Log.w(tag, buildMsg(msg));
        }
    }

    public static void w(String tag, String msg, Exception e) {
        if(LOG_ENABLE) {
            Log.w(tag, buildMsg(msg), e);
        }
    }

    public static void e(String tag, String msg) {
        if(LOG_ENABLE) {
            Log.e(tag, buildMsg(msg));
        }
    }

    public static void e(String tag, String msg, Exception e) {
        if(LOG_ENABLE) {
            Log.e(tag, buildMsg(msg), e);
        }
    }

    public static void e(String tag, String msg, Throwable thr) {
        if(LOG_ENABLE) {
            Log.e(tag, msg, thr);
        }
    }

    public static void v(Object obj) {
        v(TAG, obj == null ? "null" : buildMsg(obj.toString()));
    }

    public static void d(Object obj) {
        d(TAG, obj == null ? "null" : buildMsg(obj.toString()));
    }

    public static void i(Object obj) {
        i(TAG, obj == null ? "null" : buildMsg(obj.toString()));
    }

    public static void w(Object obj) {
        w(TAG, obj == null ? "null" : buildMsg(obj.toString()));
    }

    public static void e(Object obj) {
        e(TAG, obj == null ? "null" : buildMsg(obj.toString()));
    }

    public static void e(Throwable thr) {
        e(TAG, "", thr);
    }

    public static void e(Object obj, Throwable thr) {
        e(TAG, obj == null ? "null" : buildMsg(obj.toString()), thr);
    }

    public static void trace(Exception e) {
        if(LOG_ENABLE) {
            if( e != null ){
//                StringWriter wr = new StringWriter();
//                PrintWriter pw = new PrintWriter(wr);
//                e.printStackTrace(pw);
            }
        }
    }


    public static void printAll(JSONObject data){
        printAll("", data);
    }

    public static void printAll(String TAG, JSONObject data){
        if(LOG_ENABLE) {
            LogUtil.i(TAG, "[>>>>> printList START <<<<<<<<]");
        }

        try {
            Iterator<String> keyList = data.keys();
            while (keyList.hasNext()) {
                String key = keyList.next();
                Object obj = data.get(key);

                if(LOG_ENABLE) {
                    LogUtil.d(TAG, "[>>>>> key : "+key +"   ==== " + obj.toString()+"]");
                }
            }
        }catch (Exception e){
            LogUtil.trace(e);
        }

        if(LOG_ENABLE) {
            LogUtil.i(TAG, "[>>>>> printList END <<<<<<<<]");
        }
    }



    public static void traceLog(Thread t, String logText){
        if(LOG_ENABLE) {
            if (t == null)
                return;
            LogUtil.d(logText + ">>>>>>>>>>>>>>>>>>>>");
            StackTraceElement elements[] = t.getStackTrace();
            for (StackTraceElement e : elements) {
                LogUtil.d(e.getClassName() + "." + e.getMethodName() + ":" + e.getLineNumber());
            }
            LogUtil.d(logText + "<<<<<<<<<<<<<<<<<<<<");
        }
    }


    public static void logJson(JSONObject obj, String gap){
        if(!LOG_ENABLE)
            return;
        if( obj == null )
            return;
        try {
            Iterator<String> keys = obj.keys();
            if (keys == null)
                return;
            d(gap + "{");
            while (keys.hasNext()) {
                String key = keys.next();
                Object value = obj.opt(key);
                if (value == null)
                    continue;
                else if (value instanceof JSONObject) {
                    d(gap + "    " + key + ":");
                    logJson((JSONObject) value, gap + "    ");
                } else if (value instanceof String) {
                    d((gap + "    " + key + ":" + value + ","));
                } else if (value instanceof JSONArray) {
                    JSONArray array = (JSONArray) value;
                    d((gap + "    " + key + ":" + array.toString() + ","));
                }
            }
            d(gap + "}");
        }catch(Exception e){ }
    }

}
