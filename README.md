## download the burp cert via browser
  openssl x509 -inform DER -in cacert.der -out ca_burp.pem
  CERT="`openssl x509 -inform PEM -subject_hash_old -in ca_burp.pem | head -1`.0"
  cp ca_burp.pem $CERT

## move 9a5ba575.0 from local to /sdcard/Documents dir android 
  ad push 9a5ba575.0 /sdcard/Documents

## gain root access
  adb shell
  su

## remount /system if necessary
  mount --bind /system /system
  mount -o rw,remount /system

## move the cert to cacerts folder
  cp /sdcard/Documents/9a5ba575.0 /system/etc/security/cacerts/
  chmod 644 9a5ba575.0 ; chmod root:root 9a5ba575.0 

## dont reboot the device, when device reboot the system certificates will restore
