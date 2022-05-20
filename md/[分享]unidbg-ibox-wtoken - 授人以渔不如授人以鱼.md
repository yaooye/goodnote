> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [bbs.pediy.com](https://bbs.pediy.com/thread-272690.htm)

> [分享]unidbg-ibox-wtoken - 授人以渔不如授人以鱼

```
package ibox;
 
import com.github.unidbg.AndroidEmulator;
import com.github.unidbg.Emulator;
import com.github.unidbg.file.FileResult;
import com.github.unidbg.file.IOResolver;
import com.github.unidbg.file.linux.AndroidFileIO;
import com.github.unidbg.linux.android.AndroidEmulatorBuilder;
import com.github.unidbg.linux.android.AndroidResolver;
import com.github.unidbg.linux.android.dvm.*;
import com.github.unidbg.linux.android.dvm.array.ByteArray;
import com.github.unidbg.memory.Memory;
import com.github.unidbg.spi.SyscallHandler;
import java.io.File;
import java.io.IOException;
import java.nio.charset.StandardCharsets;
 
 
public class TigerTallyAPI extends AbstractJni implements IOResolver {
    private final AndroidEmulator emulator;
    private final VM vm;
    public TigerTallyAPI(String apkPath) {
        emulator = AndroidEmulatorBuilder.for64Bit().build();
        SyscallHandler syscallHandler =
                emulator.getSyscallHandler();
        syscallHandler.setVerbose(true);
        syscallHandler.addIOResolver(this);
        Memory memory = emulator.getMemory();
        memory.setLibraryResolver(new AndroidResolver(23));
        vm = emulator.createDalvikVM(new File(apkPath));
        vm.setJni(this);
        vm.setVerbose(true);
 
    }
    public static void main(String[] args) {
        TigerTallyAPI tigerTallyAPI = new TigerTallyAPI("D:\\apk\\ibox.apk");
        AndroidEmulator emulator = tigerTallyAPI.emulator;
        DalvikModule dalvikModule = tigerTallyAPI.vm.loadLibrary(new File("D:\\apk\\libtiger_tally.so"), true);
        dalvikModule.callJNI_OnLoad(emulator);
        VM vm = tigerTallyAPI.vm;
        DvmClass dvmClass = vm.resolveClass("com/aliyun/TigerTally/TigerTallyAPI");
        dvmClass.callStaticJniMethodObject(emulator,"_genericNt1(I)I",2);
        dvmClass.callStaticJniMethodObject(emulator,"_genericNt2(ILjava/lang/String;)I",2,new StringObject(vm,"EWA40T3eMNVkLmj8Ur9CuQExbcOti8c3yd-I8xDkLhvphNMuRujkY7V6lKbvAtE2qXa4kTWSnXmo0HXfuUXRgyFNXYwhwvvf7yUYQ-DjWjAa34fjA9yJCam4Llddmcu3D8BQKw4gR-nkYzzOx0uGj9OkfgUHoFxF00akZNyeMrs="));
        DvmObject dvmObject = dvmClass.callStaticJniMethodObject(emulator,"_genericNt3(I[B)Ljava/lang/String;",2,new ByteArray(vm,"{\"phoneNumber\":\"18888888888\",\"code\":\"188888\"}".getBytes(StandardCharsets.UTF_8)));
        System.out.println(dvmObject.getValue().toString());
        tigerTallyAPI.destroy();
 
    }
    @Override
    public DvmObject callStaticObjectMethodV(BaseVM vm, DvmClass dvmClass, String signature, VaList vaList) {
        switch (signature){
            case "com/aliyun/TigerTally/A->ct()Landroid/content/Context;":
                return vm.resolveClass("android/app/Application",vm.resolveClass("android/content/ContextWrapper",vm.resolveClass("android/content/Context"))).newObject(signature);
             case "com/aliyun/TigerTally/A->pb(Ljava/lang/String;[B)Ljava/lang/String;":
                 return new StringObject(vm,"wS8O4RYy64fZSJqmsPYWVT3K5+hweouz0YPvsxAs7x1mfWj0mqidyOwOBffV+mDcI9L0i2JLGp3YHbJYhxir0A==");
               }
        return super.callStaticObjectMethodV(vm, dvmClass, signature, vaList);
    }
 
    @Override
    public DvmObject dvmObject, String signature, VaList vaList) {
        switch (signature){
            case "android/content/pm/PackageManager->getApplicationInfo(Ljava/lang/String;I)Landroid/content/pm/ApplicationInfo;":
                return vm.resolveClass("Landroid/content/pm/ApplicationInfo;").newObject(signature);
            case "android/content/pm/PackageManager->getApplicationLabel(Landroid/content/pm/ApplicationInfo;)Ljava/lang/CharSequence;":
                return new StringObject(vm,"Ljava/lang/CharSequence;");
            case "android/app/Application->getFilesDir()Ljava/io/File;":
                return vm.resolveClass("Ljava/io/File;");
            case "java/lang/String->getAbsolutePath()Ljava/lang/String;":
                return new StringObject(vm,"Ljava/lang/String;");
            case "android/app/Application->getSharedPreferences(Ljava/lang/String;I)Landroid/content/SharedPreferences;":
                return vm.resolveClass("Landroid/content/SharedPreferences;");
            case "java/lang/Class->getAbsolutePath()Ljava/lang/String;":
                return new StringObject(vm,"Ljava/lang/String;");
        }
        return super.callObjectMethodV(vm, dvmObject, signature, vaList);
    }
    @Override
    public DvmObject getStaticObjectField(BaseVM vm, DvmClass dvmClass, String signature) {
        switch (signature){
            case "android/os/Build->BRAND:Ljava/lang/String;":
                return new StringObject(vm,"Ljava/lang/String;");
            case "android/os/Build->MODEL:Ljava/lang/String;":
                return new StringObject(vm,"Ljava/lang/String;");
            case "android/os/Build$VERSION->RELEASE:Ljava/lang/String;":
                return new StringObject(vm,"Ljava/lang/String;");
            case "android/os/Build->DEVICE:Ljava/lang/String;":
                return new StringObject(vm,"Ljava/lang/String;");
        }
        return super.getStaticObjectField(vm,dvmClass,signature);
    }
    public void destroy() {
        try {
            emulator.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    @Override
    public FileResult resolve(Emulator emulator, String pathname, int oflags) {
        return null;
    }
} 
```

![](https://bbs.pediy.com/upload/attach/202205/890609_36RD267QYMNV828.png)  
![](https://bbs.pediy.com/upload/attach/202205/890609_S7NVCHX3AKRWGYX.png)

[【公告】 讲师招募 | 全新 “预付费” 模式，不想来试试吗？](https://bbs.pediy.com/thread-271621.htm)

最后于 2022-5-10 15:15 被乌云科技团队编辑 ，原因：

[#逆向分析](forum-161-1-118.htm) [#协议分析](forum-161-1-120.htm) [#其他](forum-161-1-129.htm)