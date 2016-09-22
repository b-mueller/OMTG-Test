# Testing for Data Storage and Privacy Issues

##Testing for Insecure Storage of Credentials and Keys (OMTG-DATAST-001)

A mobile application should not store any sensitive information like credentials, passwords, usernames or encryption keys. Even by using security controls and mechanisms offered by the OS as a best practice to protect the information, it can be recovered on a rooted device. It should be remembered that the confidentiality of sensitive information stored locally on a device cannot be guaranteed. 
Should business requirements or other factors outside the security domain dictate that certain sensitive information is kept on the device there are several best practices available on the OS level that should be applied to make it harder for attackers to retrieve sensitive information. 


## Android

### White-box Testing

- Files with permissions of MODE_WORLD_READABLE or MODE_WORLD_WRITABLE are accessible even though it is stored in the app’s private data directory. WRITE_EXTERNAL_STORAGE or READ_EXTERNAL_STORAGE app permission allows access to the external phone storage which is world readable.

        egrep -irn "MODE_WORLD_READABLE|MODE_WORLD_WRITABLE|WRITE_EXTERNAL_STORAGE|READ_EXTERNAL_STORAGE" . 

- Below listed functions read from or write to the internal app directory.

        egrep -irn "openFileOutput(|createTempFile(|openFile(|getFilesDir(|getCacheDir(" . 
        
-  getExternalFilesDir() and getExternalFilesDirs() will create directory on the external storage. Files created in those directories can be only read by the app itself. getExternalStoragePublicDirectory() will create a directory that is world rw.

        egrep -irn "getExternalStoragePublicDirectory(|getExternalFilesDir(|getExternalFilesDirs(" . 

### Black-box Testing

#### Analyse the app directory 

 1. Use the mobile app extensively so that all functionality is at least triggered once.
  
 2. Download the mobile app data directory from  /data/data/com.example.appname and the global storage directory /sdcard/ . 
 
 3. Look for sensitive data such as credentials, passwords, usernames, encryption keys or any other information that could be classified as sensitive in all downloaded files. 

#### Profile file system calls  



### Remediation

Sensitive information should not be stored on the device. Instead, perform initial authentication using credentials supplied by the user, and then use a short-lived, service-specific authorization token (session token).
If credentials, keys or other sensitive information need to be stored locally use the KeyChain or KeyStore according to your requirements.

- KeyChain: Use the KeyChain API when you want system-wide credentials. When an app requests the use of any credential through the KeyChain API, users get to choose, through a system-provided UI, which of the installed credentials an app can access. This allows several apps to use the same set of credentials with user consent.

- Keystore: Use the Android Keystore provider to let an individual app store its own credentials that only the app itself can access. This provides a way for apps to manage credentials that are usable only by itself while providing the same security benefits that the KeyChain API provides for system-wide credentials. This method requires no user interaction to select the credentials.


### References

[OWASP Mobile TOP 10: Insecure Data Storage](https://www.owasp.org/index.php/Mobile_Top_10_2014-M2)

[Android KeyChain Documentation](http://developer.android.com/reference/android/security/KeyChain.html)

[Android KeyStore System Documentation](http://developer.android.com/training/articles/keystore.html)

[Android Storage Documentation](https://developer.android.com/training/basics/data-storage/index.html)


## IOS

### White-box Testing

### Black-box Testing

### Remediation

The IOS Keychain Services API provides the most secure way to store small amounts of data on a device. By making a single call to this API, an app can store small bits of secret information on a keychain, from which the app can later retrieve the information—also with a single call. The keychain secures data by encrypting it before storing it in the file system, relieving you of the need to implement complicated encryption algorithms. The system also carefully controls access to stored items. The entire keychain can be locked, meaning no one can decrypt its protected contents until it is unlocked with a master password. Even with an unlocked keychain, the system’s Keychain Access policy ensures that only authorized apps gain access to a given item in the keychain. In the simplest case, the app that created an item is the only one that can access it later. 
