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
package com.example.privacy

import android.content.Context
import androidx.security.crypto.EncryptedSharedPreferences
import androidx.security.crypto.MasterKey

class CryptoVault(context: Context) {
    private val masterKey = MasterKey.Builder(context)
        .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
        .build()

    private val preferences = EncryptedSharedPreferences.create(
        context,
        "private_gate_secure_prefs",
        masterKey,
        EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
        EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
    )

    fun savePrivateSetting(key: String, value: String) {
        preferences.edit().putString(key, value).apply()
    }

    fun readPrivateSetting(key: String): String? {
        return preferences.getString(key, null)
    }

    fun clear() {
        preferences.edit().clear().apply()
    }
}androidx.security.crypto.EncryptedSharedPreferencesandroidx.security.crypto.MasterKeypackage com.example.privacy.biometric

import android.content.Context
import androidx.biometric.BiometricManager
import androidx.biometric.BiometricPrompt
import androidx.core.content.ContextCompat
import androidx.fragment.app.FragmentActivity

class BiometricGate(private val activity: FragmentActivity) {

    fun isAvailable(): Int {
        return BiometricManager.from(activity).canAuthenticate(
            BiometricManager.Authenticators.BIOMETRIC_STRONG
        )
    }

    fun authenticate(
        title: String = "Unlock PrivateGate",
        subtitle: String = "Use your enrolled biometric",
        onSuccess: () -> Unit,
        onFailure: (String) -> Unit
    ) {
        val status = isAvailable()
        if (status != BiometricManager.BIOMETRIC_SUCCESS) {
            onFailure("Strong biometric authentication is unavailable")
            return
        }

        val executor = ContextCompat.getMainExecutor(activity)
        val prompt = BiometricPrompt(
            activity,
            executor,
            object : BiometricPrompt.AuthenticationCallback() {
                override fun onAuthenticationSucceeded(
                    result: BiometricPrompt.AuthenticationResult
                ) {
                    onSuccess()
                }

                override fun onAuthenticationError(
                    errorCode: Int,
                    errString: CharSequence
                ) {
                    onFailure(errString.toString())
                }

                override fun onAuthenticationFailed() {
                    onFailure("Biometric not recognized")
                }
            }
        )

        val promptInfo = BiometricPrompt.PromptInfo.Builder()
            .setTitle(title)
            .setSubtitle(subtitle)
            .setAllowedAuthenticators(
                BiometricManager.Authenticators.BIOMETRIC_STRONG
            )
            .setNegativeButtonText("Cancel")
            .setConfirmationRequired(true)
            .build()

        prompt.authenticate(promptInfo)
    }
}androidx.biometric.BiometricPrompt<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="Theme.PrivateGate"
        parent="Theme.Material3.DayNight.NoActionBar" />
</resources>http://schemas.android.com/apk/res/android
package com.example.privacy.biometric

import android.content.Context
import androidx.biometric.BiometricManager
import androidx.biometric.BiometricPrompt
import androidx.core.content.ContextCompat
import androidx.fragment.app.FragmentActivity

class BiometricGate(private val activity: FragmentActivity) {

    fun isAvailable(): Int {
        return BiometricManager.from(activity).canAuthenticate(
            BiometricManager.Authenticators.BIOMETRIC_STRONG
        )
    }

    fun authenticate(
        title: String = "Unlock PrivateGate",
        subtitle: String = "Use your enrolled biometric",
        onSuccess: () -> Unit,
        onFailure: (String) -> Unit
    ) {
        val status = isAvailable()
        if (status != BiometricManager.BIOMETRIC_SUCCESS) {
            onFailure("Strong biometric authentication is unavailable")
            return
        }

        val executor = ContextCompat.getMainExecutor(activity)
        val prompt = BiometricPrompt(
            activity,
            executor,
            object : BiometricPrompt.AuthenticationCallback() {
                override fun onAuthenticationSucceeded(
                    result: BiometricPrompt.AuthenticationResult
                ) {
                    onSuccess()
                }

                override fun onAuthenticationError(
                    errorCode: Int,
                    errString: CharSequence
                ) {
                    onFailure(errString.toString())
                }

                override fun onAuthenticationFailed() {
                    onFailure("Biometric not recognized")
                }
            }
        )

        val promptInfo = BiometricPrompt.PromptInfo.Builder()
            .setTitle(title)
            .setSubtitle(subtitle)
            .setAllowedAuthenticators(
                BiometricManager.Authenticators.BIOMETRIC_STRONG
            )
            .setNegativeButtonText("Cancel")
            .setConfirmationRequired(true)
            .build()

        prompt.authenticate(promptInfo)
    }
}package com.example.privacy.biometric

import android.content.Context
import androidx.biometric.BiometricManager
import androidx.biometric.BiometricPrompt
import androidx.core.content.ContextCompat
import androidx.fragment.app.FragmentActivity

class BiometricGate(private val activity: FragmentActivity) {

    fun isAvailable(): Int {
        return BiometricManager.from(activity).canAuthenticate(
            BiometricManager.Authenticators.BIOMETRIC_STRONG
        )
    }

    fun authenticate(
        title: String = "Unlock PrivateGate",
        subtitle: String = "Use your enrolled biometric",
        onSuccess: () -> Unit,
        onFailure: (String) -> Unit
    ) {
        val status = isAvailable()
        if (status != BiometricManager.BIOMETRIC_SUCCESS) {
            onFailure("Strong biometric authentication is unavailable")
            return
        }

        val executor = ContextCompat.getMainExecutor(activity)
        val prompt = BiometricPrompt(
            activity,
            executor,
            object : BiometricPrompt.AuthenticationCallback() {
                override fun onAuthenticationSucceeded(
                    result: BiometricPrompt.AuthenticationResult
                ) {
                    onSuccess()
                }

                override fun onAuthenticationError(
                    errorCode: Int,
                    errString: CharSequence
                ) {
                    onFailure(errString.toString())
                }

                override fun onAuthenticationFailed() {
                    onFailure("Biometric not recognized")
                }
            }
        )

        val promptInfo = BiometricPrompt.PromptInfo.Builder()
            .setTitle(title)
            .setSubtitle(subtitle)
            .setAllowedAuthenticators(
                BiometricManager.Authenticators.BIOMETRIC_STRONG
            )
            .setNegativeButtonText("Cancel")
            .setConfirmationRequired(true)
            .build()

        prompt.authenticate(promptInfo)
    }
}
package com.example.privacy.biometric

import android.content.Context
import androidx.biometric.BiometricManager
import androidx.biometric.BiometricPrompt
import androidx.core.content.ContextCompat
import androidx.fragment.app.FragmentActivity

class BiometricGate(private val activity: FragmentActivity) {

    fun isAvailable(): Int {
        return BiometricManager.from(activity).canAuthenticate(
            BiometricManager.Authenticators.BIOMETRIC_STRONG
        )
    }

    fun authenticate(
        title: String = "Unlock PrivateGate",
        subtitle: String = "Use your enrolled biometric",
        onSuccess: () -> Unit,
        onFailure: (String) -> Unit
    ) {
        val status = isAvailable()
        if (status != BiometricManager.BIOMETRIC_SUCCESS) {
            onFailure("Strong biometric authentication is unavailable")
            return
        }

        val executor = ContextCompat.getMainExecutor(activity)
        val prompt = BiometricPrompt(
            activity,
            executor,
            object : BiometricPrompt.AuthenticationCallback() {
                override fun onAuthenticationSucceeded(
                    result: BiometricPrompt.AuthenticationResult
                ) {
                    onSuccess()
                }

                override fun onAuthenticationError(
                    errorCode: Int,
                    errString: CharSequence
                ) {
                    onFailure(errString.toString())
                }

                override fun onAuthenticationFailed() {
                    onFailure("Biometric not recognized")
                }
            }
        )

        val promptInfo = BiometricPrompt.PromptInfo.Builder()
            .setTitle(title)
            .setSubtitle(subtitle)
            .setAllowedAuthenticators(
                BiometricManager.Authenticators.BIOMETRIC_STRONG
            )
            .setNegativeButtonText("Cancel")
            .setConfirmationRequired(true)
            .build()

        prompt.authenticate(promptInfo)
    }
