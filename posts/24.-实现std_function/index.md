# 实现std::function




```cpp
#include &lt;iostream&gt;
#include &lt;functional&gt;

int myprint()
{
    std::cout &lt;&lt; __func__ &lt;&lt; std::endl;
    return 1;
}

namespace Mico {
    template&lt;typename RetType, typename... Args&gt;
    class Functional {};

    template&lt;typename RetType, typename... Args&gt;
    class Functional&lt;RetType(Args...)&gt; {
        using FuncPtrType = RetType (*)(Args...);
    public:
        Functional(FuncPtrType func) : _func(func)
        {

        }

        RetType operator()(Args... args) 
        {   
            std::cout &lt;&lt; __func__ &lt;&lt; std::endl;
            return _func(args...);
        }

    private:
        FuncPtrType _func;
    };
}


int main ()
{
    Mico::Functional&lt;int()&gt; fp{myprint};
    fp();
}
```

上面的代码没有做到类型擦除？实际上只是相当于函数指针？




```cpp
    template&lt;typename RetType, typename... Args&gt;
    class Functional {};

    // 偏特化
    template&lt;typename RetType, typename... Args&gt;
    class Functional&lt;RetType(Args...)&gt; {
        using FuncPtrType = RetType (*)(Args...);

    class ICallable {
    public:
        virtual ~ICallable() = default;
        virtual RetType Invoke(Args...) = 0;
    };

    template &lt;typename T&gt;
    class CallableT : public ICallable {
    public:
        CallableT(const T&amp; t)
            : t_(t) {
        }

        ~CallableT() override = default;

        RetType Invoke(Args... args) override {
            return t_(args...);
        }

    private:
        T t_;
    };

    public:
        template &lt;typename T&gt;
        Functional&amp; operator=(T t) {
            callable_ = std::make_unique&lt;CallableT&lt;T&gt;&gt;(t);
            return *this;
        }

        RetType operator()(Args... args) 
        {   
            std::cout &lt;&lt; __func__ &lt;&lt; std::endl;
            return callable_-&gt;Invoke(args...);
        }

    private:
        std::unique_ptr&lt;ICallable&gt; callable_;
    };

```

---

> Author: Kristoffer  
> URL: http://localhost:1313/posts/24.-%E5%AE%9E%E7%8E%B0std_function/  

