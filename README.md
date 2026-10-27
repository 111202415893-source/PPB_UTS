# Laporan UTS 20261 — HRIS Mini (Pengajuan Cuti)

Aplikasi internal **HRIS mini** untuk mengelola pengajuan cuti karyawan. Aplikasi ini dikembangkan dari *starter code* UTS untuk memenuhi dua kebutuhan utama Tim HR: fitur persetujuan cuti (Approve/Reject) dan fitur penyaringan data (Filter Status & Tipe). Seluruh fitur dikerjakan tanpa menggunakan *package* pihak ketiga.

## Cara Menjalankan Aplikasi

```bash
flutter pub get
flutter run -d chrome        # mode web
# atau (Android emulator):
flutter run -d <device-id>

***
Laporan UTS 20261 — HRIS Mini (Pengajuan Cuti)

Aplikasi internal HRIS mini untuk mengelola pengajuan cuti karyawan. Aplikasi ini dikembangkan dari starter code UTS untuk memenuhi dua kebutuhan utama Tim HR: fitur persetujuan cuti (Approve/Reject) dan fitur penyaringan data (Filter Status & Tipe). Seluruh fitur dikerjakan tanpa menggunakan package pihak ketiga.

Cara Menjalankan Aplikasi

flutter pub get
flutter run -d chrome
atau (Android emulator):
flutter run -d (device-id)

(Catatan: Folder platform dapat dibuat dengan perintah flutter create . --platforms=android,web jika belum ada).

Daftar Perbaikan & Fitur Baru

Berikut adalah rangkuman perubahan, penambahan kode, dan alasan di balik pengerjaan Tugas 1 dan Tugas 2:

1. Fitur Persetujuan & Penolakan Cuti (Approve / Reject)
Apa yang dikerjakan: Menambahkan tombol "Setujui" dan "Tolak" pada layar detail pengajuan (LeaveDetailScreen). Status baru kemudian dikirim kembali ke layar daftar (LeaveListScreen) dan kartu (LeaveCard) diperbarui secara otomatis.
Alasan: Tim HR membutuhkan cara untuk memproses pengajuan cuti. Fitur ini diwajibkan untuk memenuhi Tugas 1. Tombol aksi hanya ditampilkan jika status pengajuan masih pending (Menunggu) sesuai instruksi soal.
File yang diubah: lib/screens/leave_detail_screen.dart (menambahkan bottomNavigationBar berisi tombol) dan lib/screens/leave_list_screen.dart (mengubah fungsi _openDetail menjadi async untuk menangkap nilai balik Navigator.pop dan mengupdate state via setState).
Teknis: Karena class LeaveRequest bersifat immutable (tidak bisa diubah langsung), status baru di-set menggunakan method copyWith(status: ...). Data di dalam list _requests kemudian dicari berdasarkan id-nya, lalu diganti dengan objek baru.

2. Fitur Filter Kombinasi Status & Tipe Cuti
Apa yang dikerjakan: Menambahkan barisan Filter Chips di bagian atas daftar (sebelum ListView). Terdapat dua baris filter: Status (Semua, Menunggu, Disetujui, Ditolak) dan Tipe (Semua, Cuti Tahunan, Sakit, Tanpa Gaji).
Alasan: Tim HR kesulitan mencari data spesifik karena semua cuti tercampur. Fitur ini memenuhi Tugas 2 soal logika filter (AND).
File yang diubah: lib/screens/leave_list_screen.dart (menambahkan state variable LeaveStatus? _statusFilter dan LeaveType? _typeFilter, serta widget _FilterBar dan _ChipRow).
Teknis: Digunakan tipe data nullable (?) dengan nilai default null untuk merepresentasikan pilihan "Semua". Proses penyaringan menggunakan method .where() pada list. Logika AND diterapkan dengan mengecek apakah _statusFilter cocok DAN _typeFilter cocok. ChoiceChip digunakan karena sesuai dengan tema aplikasi dan memiliki indikator selected (warna biru gelap penuh) sehingga jelas mana filter yang sedang aktif.

3. Fitur Tampilan Data Kosong (Empty State)
Apa yang dikerjakan: Membuat widget baru bernama EmptyState dan menampilkannya saat hasil filter tidak menemukan data yang cocok (contoh kasus: filter Ditolak + Sakit).
Alasan: Agar aplikasi tidak terlihat blank atau error saat pengguna memilih kombinasi filter yang tidak memiliki hasil. Fitur ini diwajibkan di Tugas 2.
File yang diubah: lib/widgets/empty_state.dart (File baru) dan lib/screens/leave_list_screen.dart (Menambahkan kondisi if sebelum memanggil ListView).

4. Penyempurnaan UI/UX (Code Quality & Estetika)
Apa yang dikerjakan: Mempercantik tampilan antarmuka agar tidak terlihat kaku.
Alasan: Memenuhi aspek penilaian Code quality (10 poin) serta memberikan user experience yang lebih baik dan pembeda dari template dasar.
Detail Perubahan:
Filter Bar: Diberi header teks kecil ("Filter Status" & "Filter Tipe Cuti") agar lebih intuitif.
Warna Chip: ChoiceChip yang aktif diberi warna colorScheme.primary dengan teks putih tebal, sedangkan yang tidak aktif dibiarkan outline.
Header Halaman: Menambahkan ikon assignment_ind_rounded di samping judul "Pengajuan Cuti" untuk menghilangkan kesan monoton.
Kartu (LeaveCard): Mengganti ikon kalender statis dengan format teks tanggal yang lebih padat dan mudah dibaca. Icon tipe cuti juga diberi badge background transparan agar terlihat lebih berwarna.

Struktur File yang Diubah

lib/screens/leave_list_screen.dart (Diubah: Filter Bar, state filter, async open detail, empty state logic)
lib/screens/leave_detail_screen.dart (Diubah: Bottom Navigation Bar untuk Approve/Reject)
lib/widgets/empty_state.dart (Baru: Widget informasi saat filter tidak menghasilkan data)

Hasil Verifikasi

Semua persyaratan UTS telah terpenuhi dan aplikasi berjalan dengan baik:
[x] flutter analyze -> No issues found (0 error/warning).
[x] flutter test -> All tests passed.
[x] Tombol Setujui & Tolak hanya muncul pada pengajuan berstatus "Menunggu".
[x] Perubahan status langsung terlihat pada chip di halaman daftar.
[x] Filter status dan tipe dapat dikombinasikan (Logika AND).
[x] Empty state berhasil muncul saat kombinasi filter tidak menghasilkan data.