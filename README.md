# secure-android
trying to make my android less sucseptable to remote mirroring type attacks on my device
app/
  build.gradle.kts
  src/main/
    AndroidManifest.xml
    java/com/example/privacy/
      MainActivity.kt
      CryptoVault.kt
      biometric/BiometricGate.kt
      network/DnsRepository.kt
      network/PrivacyProfile.kt
server/
  biometrics.py
README.md
LICENSE
.gitignore