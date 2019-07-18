# 函数节流与防抖的起因
  > 前端用户输入、鼠标滚动等无限加载事件都会发起ajax请求，并重新渲染dom,这种频繁的操作会引起不必要的资源消耗。

# 函数节流
  > 节流。就是拧紧水龙头让水少流一点，但是不是不让水流了，即限制请求的频率，减少调用次数。如滚动条事件，滚动获取更多内容的场景。

  ```
  函数实现
    /** wait: 执行的频率，func: 对应的处理函数,
    **  内部需要一个lastTime 变量记录上一次执行的时间
    **/
    function throttle (func, wait) {
      let lastTime = null
      let timeout
      return function () {
        let context = this
        let now = new Date()
        // 如果上次执行的时间和这次触发的时间大于一个执行周期，则执行
        if (now - lastTime - wait > 0) {
          // 如果之前有了定时任务则清除
          if (timeout) {
            clearTimeout(timeout)
            timeout = null
          }
          func.apply(context, arguments)
          lastTime = now
        } else if (!timeout) {
          timeout = setTimeout(() => {
            // 改变执行上下文环境
            func.apply(context, arguments)
          }, wait)
        }
      }
    }
  函数调用
    let throttleRun = throttle(() => {
    }, 2000)
  ```

# 函数防抖
  > 防抖。在一定时间段的连续函数调用，只让其执行判定时间内的最后一次事件。如搜索引擎查询模糊搜索内容等场景。

  ```
  函数实现
    function debounce (func, wait) {
      let lastTime = null
      let timeout
      return function () {
        let context = this
        let now = new Date()
        // 判定不是一次抖动
        if (now - lastTime - wait > 0) {
          setTimeout(() => {
            func.apply(context, arguments)
          }, wait)
        } else {
          if (timeout) {
            clearTimeout(timeout)
            timeout = null
          }
          timeout = setTimeout(() => {
            func.apply(context, arguments)
          }, wait)
        }
        // 注意这里lastTime是上次的触发时间
        lastTime = now
      }
    }
  函数调用
    let debounceRun = debounce(() => {
    }, 2000)
  ```

# 总结
  * 函数防抖和函数节流都是防止某一时间频繁触发，但这两兄弟之间的原理却不一样。
  * 函数防抖是某一段时间内只执行一次，而函数节流是间隔时间执行。