# Study Flow release key

This folder is for the local release signing keystore only. The actual `.jks`
file is ignored by Git and must never be committed.

Create one from the `android` directory after copying
`keystore.properties.example` to `keystore.properties`:

```powershell
keytool -genkeypair -v -keystore keystore/study-flow-release.jks -alias studyflow -keyalg RSA -keysize 4096 -validity 10000
```

Use the same passwords in `keystore.properties`. Store an encrypted backup of
both the keystore and the passwords outside the repository. Without this exact
keystore, Android will not accept future signed updates to the installed app.
