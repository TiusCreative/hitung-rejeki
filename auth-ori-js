/**
 * auth.js - Modul Otentikasi Terpisah untuk Aplikasi Hitung Rejeki Weton
 * Daftar Email & Password Terotorisasi:
 * 1. tiuss168@gmal.com          - password: liedia99
 * 2. tiuss168@gmail.com         - password: liedia99
 * 3. tiuss75@gmail.com          - password: liedia99
 * 4. creativecortex168@gmail.com - password: liedia99
 * 5. suyono@gmail.com           - password: suyono
 *
 * Cara mengubah password:
 *   Cari objek USER_CREDENTIALS di bawah ini, lalu ganti nilai
 *   pada key email yang ingin diubah. Contoh:
 *   "tiuss75@gmail.com": "passwordbaru"
 */

// ============================================================
// KONFIGURASI EMAIL & PASSWORD — Ubah di sini jika diperlukan
// ============================================================
const USER_CREDENTIALS = {
    "tiuss168@gmal.com":           "liedia99",
    "tiuss168@gmail.com":          "liedia99",
    "tiuss75@gmail.com":           "liedia99",
    "creativecortex168@gmail.com": "liedia99",
    "suyono@gmail.com":            "suyono"
};

const ALLOWED_EMAILS = Object.keys(USER_CREDENTIALS);

const AUTH_STORAGE_KEY = "hitung_rejeki_user_session";

/**
 * Memeriksa apakah email diizinkan untuk login
 * @param {string} email 
 * @returns {boolean}
 */
function isEmailAllowed(email) {
    if (!email) return false;
    const cleanEmail = email.trim().toLowerCase();
    return ALLOWED_EMAILS.includes(cleanEmail);
}

/**
 * Mengambil data user yang sedang login
 * @returns {object|null}
 */
function getLoggedInUser() {
    try {
        const stored = localStorage.getItem(AUTH_STORAGE_KEY) || sessionStorage.getItem(AUTH_STORAGE_KEY);
        if (!stored) return null;
        return JSON.parse(stored);
    } catch (e) {
        console.error("Gagal membaca sesi otentikasi:", e);
        return null;
    }
}

/**
 * Melakukan proses login
 * @param {string} email 
 * @param {string} password 
 * @param {boolean} rememberMe 
 * @returns {object} { success: boolean, message?: string, user?: object }
 */
function loginUser(email, password, rememberMe = true) {
    if (!email || !email.trim()) {
        return { success: false, message: "Harap masukkan alamat email Anda." };
    }
    if (!password || !password.trim()) {
        return { success: false, message: "Harap masukkan kata sandi Anda." };
    }

    const cleanEmail = email.trim().toLowerCase();

    if (!isEmailAllowed(cleanEmail)) {
        return { 
            success: false, 
            message: "Email ini tidak memiliki hak akses. Gunakan salah satu email terdaftar!" 
        };
    }

    // Validasi Password
    const correctPassword = USER_CREDENTIALS[cleanEmail];
    if (password !== correctPassword) {
        return { success: false, message: "Kata sandi salah. Silakan coba lagi!" };
    }

    // Nama tampilan berdasarkan email
    let displayName = cleanEmail.split('@')[0];
    if (cleanEmail.includes('tiuss168')) displayName = 'Tius 168';
    else if (cleanEmail.includes('tiuss75')) displayName = 'Tius 75';
    else if (cleanEmail.includes('creativecortex')) displayName = 'Creative Cortex';
    else if (cleanEmail.includes('suyono')) displayName = 'Suyono';

    const userData = {
        email: cleanEmail,
        displayName: displayName,
        loginAt: new Date().toISOString()
    };

    try {
        const storage = rememberMe ? localStorage : sessionStorage;
        storage.setItem(AUTH_STORAGE_KEY, JSON.stringify(userData));
        return { success: true, user: userData };
    } catch (e) {
        return { success: false, message: "Gagal menyimpan sesi login di browser." };
    }
}

/**
 * Melakukan logout pengguna dan mengalihkan ke halaman login
 */
function logoutUser() {
    try {
        localStorage.removeItem(AUTH_STORAGE_KEY);
        sessionStorage.removeItem(AUTH_STORAGE_KEY);
    } catch (e) {
        console.error("Gagal menghapus sesi login:", e);
    }
    window.location.href = "login.html";
}

/**
 * Guard untuk halaman terlindungi (seperti index.html)
 * Mengalihkan ke login.html jika pengguna belum terotentikasi.
 */
function checkAuthGuard() {
    const user = getLoggedInUser();
    if (!user) {
        window.location.href = "login.html";
    }
    return user;
}

/**
 * Guard untuk halaman login (login.html)
 * Mengalihkan ke index.html jika pengguna sudah terotentikasi.
 */
function checkAlreadyLoggedIn() {
    const user = getLoggedInUser();
    if (user) {
        window.location.href = "index.html";
    }
}
