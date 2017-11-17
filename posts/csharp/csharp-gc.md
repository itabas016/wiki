---
title: "CSharp GC"
layout: post
date: 2017-11-09 21:11:00
toc: true
---

## 垃圾回收算法 ##

## Finalize原理 ##

实现方式是`~ClassName()`, 一般会调用Win32 `CloseHandle`函数，并向该函数传递本地资源的句柄。

* 使用`CriticalFinalizerObject`类型确保终结

看具体实现：
``` csharp
using System.Runtime.InteropServices;

namespace System.Runtime.ConstrainedExecution
{
    //
    // Summary:
    //     Ensures that all finalization code in derived classes is marked as critical.
    [ComVisible(true)]
    public abstract class CriticalFinalizerObject
    {
        //
        // Summary:
        //     Initializes a new instance of the System.Runtime.ConstrainedExecution.CriticalFinalizerObject
        //     class.
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.MayFail)]
        protected CriticalFinalizerObject();

        //
        // Summary:
        //     Releases all the resources used by the System.Runtime.ConstrainedExecution.CriticalFinalizerObject
        //     class.
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.Success)]
        ~CriticalFinalizerObject();
    }
}
```
* `SafeHandle`类型及其派生类型

``` csharp
using System.Runtime.ConstrainedExecution;
using System.Security;

namespace System.Runtime.InteropServices
{
    //
    // Summary:
    //     Represents a wrapper class for operating system handles. This class must be inherited.
    [SecurityCritical]
    public abstract class SafeHandle : CriticalFinalizerObject, IDisposable
    {
        //
        // Summary:
        //     Specifies the handle to be wrapped.
        [ForceTokenStabilizationAttribute]
        protected IntPtr handle;

        //
        // Summary:
        //     Initializes a new instance of the System.Runtime.InteropServices.SafeHandle class
        //     with the specified invalid handle value.
        //
        // Parameters:
        //   invalidHandleValue:
        //     The value of an invalid handle (usually 0 or -1). Your implementation of System.Runtime.InteropServices.SafeHandle.IsInvalid
        //     should return true for this value.
        //
        //   ownsHandle:
        //     true to reliably let System.Runtime.InteropServices.SafeHandle release the handle
        //     during the finalization phase; otherwise, false (not recommended).
        //
        // Exceptions:
        //   T:System.TypeLoadException:
        //     The derived class resides in an assembly without unmanaged code access permission.
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.MayFail)]
        protected SafeHandle(IntPtr invalidHandleValue, bool ownsHandle);

        //
        // Summary:
        //     Frees all resources associated with the handle.
        [SecuritySafeCritical]
        ~SafeHandle();

        //
        // Summary:
        //     Gets a value indicating whether the handle is closed.
        //
        // Returns:
        //     true if the handle is closed; otherwise, false.
        public bool IsClosed { get; }
        //
        // Summary:
        //     When overridden in a derived class, gets a value indicating whether the handle
        //     value is invalid.
        //
        // Returns:
        //     true if the handle value is invalid; otherwise, false.
        public abstract bool IsInvalid { get; }

        //
        // Summary:
        //     Marks the handle for releasing and freeing resources.
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.Success)]
        [SecurityCritical]
        public void Close();
        //
        // Summary:
        //     Manually increments the reference counter on System.Runtime.InteropServices.SafeHandle
        //     instances.
        //
        // Parameters:
        //   success:
        //     true if the reference counter was successfully incremented; otherwise, false.
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.MayFail)]
        [SecurityCritical]
        public void DangerousAddRef(ref bool success);
        //
        // Summary:
        //     Returns the value of the System.Runtime.InteropServices.SafeHandle.handle field.
        //
        // Returns:
        //     An IntPtr representing the value of the System.Runtime.InteropServices.SafeHandle.handle
        //     field. If the handle has been marked invalid with System.Runtime.InteropServices.SafeHandle.SetHandleAsInvalid,
        //     this method still returns the original handle value, which can be a stale value.
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.Success)]
        [TargetedPatchingOptOut("Performance critical to inline across NGen image boundaries")]
        public IntPtr DangerousGetHandle();
        //
        // Summary:
        //     Manually decrements the reference counter on a System.Runtime.InteropServices.SafeHandle
        //     instance.
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.Success)]
        [SecurityCritical]
        public void DangerousRelease();
        //
        // Summary:
        //     Releases all resources used by the System.Runtime.InteropServices.SafeHandle
        //     class.
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.Success)]
        [SecuritySafeCritical]
        public void Dispose();
        //
        // Summary:
        //     Marks a handle as no longer used.
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.Success)]
        [SecurityCritical]
        public void SetHandleAsInvalid();
        //
        // Summary:
        //     Releases the unmanaged resources used by the System.Runtime.InteropServices.SafeHandle
        //     class specifying whether to perform a normal dispose operation.
        //
        // Parameters:
        //   disposing:
        //     true for a normal dispose operation; false to finalize the handle.
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.Success)]
        [SecurityCritical]
        protected virtual void Dispose(bool disposing);
        //
        // Summary:
        //     When overridden in a derived class, executes the code required to free the handle.
        //
        // Returns:
        //     true if the handle is released successfully; otherwise, in the event of a catastrophic
        //     failure, false. In this case, it generates a releaseHandleFailed MDA Managed
        //     Debugging Assistant.
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.Success)]
        protected abstract bool ReleaseHandle();
        //
        // Summary:
        //     Sets the handle to the specified pre-existing handle.
        //
        // Parameters:
        //   handle:
        //     The pre-existing handle to use.
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.Success)]
        [TargetedPatchingOptOut("Performance critical to inline across NGen image boundaries")]
        protected void SetHandle(IntPtr handle);
    }
}
```

它派生自`CriticalFinalizerObject`, 这会被CLR特殊对待。并且是抽象类，派生类将重写抽象方法`ReleaseHandle`以及抽象属性`IsInvalid`的get方法。

例子：
``` csharp
using System.Runtime.ConstrainedExecution;
using System.Runtime.InteropServices;
using System.Security;

namespace Microsoft.Win32.SafeHandles
{
    //
    // Summary:
    //     Provides a base class for Win32 safe handle implementations in which the value
    //     of either 0 or -1 indicates an invalid handle.
    [SecurityCritical]
    public abstract class SafeHandleZeroOrMinusOneIsInvalid : SafeHandle
    {
        //
        // Summary:
        //     Initializes a new instance of the Microsoft.Win32.SafeHandles.SafeHandleZeroOrMinusOneIsInvalid
        //     class, specifying whether the handle is to be reliably released.
        //
        // Parameters:
        //   ownsHandle:
        //     true to reliably release the handle during the finalization phase; false to prevent
        //     reliable release (not recommended).
        [ReliabilityContract(Consistency.WillNotCorruptState, Cer.MayFail)]
        protected SafeHandleZeroOrMinusOneIsInvalid(bool ownsHandle);

        //
        // Summary:
        //     Gets a value that indicates whether the handle is invalid.
        //
        // Returns:
        //     true if the handle is not valid; otherwise, false.
        public override bool IsInvalid { get; }
    }
}

///SafeFileHandle
using System;
using System.Security;

namespace Microsoft.Win32.SafeHandles
{
    [SecurityCritical]
    public sealed class SafeFileHandle : SafeHandleZeroOrMinusOneIsInvalid
    {
        public SafeFileHandle(IntPtr preexistingHandle, bool ownsHandle);

        [SecurityCritical]
        protected override bool ReleaseHandle();
    }
}
```

## Finalize调用 ##

* 第0代满，垃圾回收会自动开始
* 显示调用`GC.Collect`静态方法
* Windows内存不足
* CLR卸载AppDomain
* CLR关闭

## Dispose模式 ##

通常`Finalize`的调用时间不能保证，并且不是公共方法不可以显示调用。一般采用`Dispose`主动强制对象清理。