1. (On physical device) Manual move the cert 
### download the burp cert via browser
openssl x509 -inform DER -in cacert.der -out ca_burp.pem
CERT="`openssl x509 -inform PEM -subject_hash_old -in ca_burp.pem | head -1`.0"
cp ca_burp.pem $CERT

### move 9a5ba575.0 from local to /sdcard/Documents dir android 
ad push 9a5ba575.0 /sdcard/Documents

### gain root access
adb shell
su

### remount /system if necessary
mount --bind /system /system
mount -o rw,remount /system

### move the cert to cacerts folder
cp /sdcard/Documents/9a5ba575.0 /system/etc/security/cacerts/
chmod 644 9a5ba575.0 ; chmod root:root 9a5ba575.0 

### dont reboot the device, when device reboot the system certificates will restore

2. (On physical or emulator device) Using MagiskTrustUserCerts 
### first of all install the cacert.der as a user
### then install AlwaysTrustUserCerts.zip via magisk modules

3. (On emulator device) Try with the first method or the second method

4. (On emulator device) Running emulator with writable system
### find avd data directory, see the avd name
### make sure the directory contain the .ini file (Pixel_6_API_31.ini)
### open PowerShell then cd to emulator path
cd C:\Users\<user>\AppData\Local\Android\Sdk\emulator
.\emulator.exe -avd Pixel_6_API_31 -writable-system
### after the emulator running next is remount the /system
./adb devices
./adb root
./adb remount
./adb reboot
./adb root
./adb remount
./adb shell
cp /sdcard/Download/9a5ba575.0 /system/etc/security/cacerts/; ls -l /system/etc/security/cacerts/
### -rw-r--r-- 1 root root 1374 2024-10-25 19:54 9a5ba575.0
### the correct cer permission, if the cert not the same you need to set the permission
chmod -R 644 /system/etc/security/cacerts/
reboot
