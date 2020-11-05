### 致命错误中间件

```golang
  defer func() {
   if err := recover(); err != nil {
    //debug.PrintStack()
    switch err.(type) {
    case runtime.Error:
     DebugStack := ""
     for _, v := range strings.Split(string(debug.Stack()), "\r") {
      DebugStack += v
     }
     glog.Channel("exception").WithFields(log.Fields{
      "requestTime": time.Now().Format("2006-01-02 15:04:05"),
      "requestURL":  c.Request.Method + "  " + c.Request.Host + c.Request.RequestURI,
      "requestUA":   c.Request.UserAgent(),
      "requestIP":   c.ClientIP(),
      "err":         errorToString(err),
      "debugStack":  DebugStack,
     }).Error("异常错误")
     break
    default:
     _ = fmt.Errorf("error:", err)
    }

    responses.Failed(c, "系统异常，请联系管理员！", errorToString(err))
   }
  }()
```
