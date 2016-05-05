ZMRAA is an implementation of the MRAA API for Zephyr project.

Supported Devices
-----------------

* Quark D2000

Adding ZMRAA to Zephyr
======================

```
cd zephyr/libs
git clone git@github.com:intel-iot-devkit/zmraa.git mraa
```

Then you need to apply a patch to zephyr to add the configuration entries
```
diff --git a/Makefile b/Makefile
index afeabb3..33bf011 100644
--- a/Makefile
+++ b/Makefile
@@ -570,6 +570,12 @@ libs-y += $(KCRYPTO_DIR)/
  ZEPHYRINCLUDE += -I$(srctree)/lib/crypto/tinycrypt/include
 endif
 
+ifdef CONFIG_MRAA
+KMRAA_DIR := lib/mraa
+libs-y += ${KMRAA_DIR}/
+ ZEPHYRINCLUDE += -I$(srctree)/lib/mraa/include
+endif
+
 ARCH = $(subst $(DQUOTE),,$(CONFIG_ARCH))
 export ARCH
 ifdef ZEPHYR_GCC_VARIANT
diff --git a/misc/Kconfig b/misc/Kconfig
index 4c05169..1d514eb 100644
--- a/misc/Kconfig
+++ b/misc/Kconfig
@@ -21,6 +21,10 @@ menu "Cryptography"
 source "lib/crypto/tinycrypt/Kconfig"
 endmenu
 
+menu "Mraa"
+source "lib/mraa/Kconfig"
+endmenu
+
 menu "Compile and Link Features"
 
 config  KERNEL_BIN_NAME
```

Then add mraa to your zephyr configuration and build your sample
```
cd zephyr/samples/hello_world/nanokernel
make BOARD=quark_d2000_crb ARCH=x86 menuconfig
```