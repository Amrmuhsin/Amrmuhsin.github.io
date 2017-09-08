---
title: Android Studio
tags:
	- Blog Intern
categories:
	- Artivisi
commands: true
date: 2017-08-16 11:35:45
---

# Belajar Android Studio

1. ArrayAdapter ListView
	Membuat ListView di Android Studio. Android listview adalah tampilan beberapa item dan bentuk list/daftar yang dapat digulir secara vertikal. Daftar item secara otomatis dimasukkan ke dalam list menggunakan Adapter yang dapat mengambil konten atau isi dari source/sumber seperti array atau database.

	Anda dapat menggunakan adapter ini jika sumber datanya adalah array. Secara default, ArrayAdapter menciptkan tampilan untuk setiap item array dengan memanggil toString() pada setiap item dan menempatkan konten dalam TextView. Jika Anda memiliki sebuah array string yang ingin ditampilkan di ListView, inisialisasi ArrayAdapter baru menggunakan konstruktor menentukan tata letak untuk setiap string dan array string.

		ArrayAdapter adapter = new ArrayAdapter(this,R.layout.ListView,StringArray);

	Setelah adapter dibuat, maka selanjutnya cukup memanggil setAdapter() pada objek ListView Anda seperti berikut

		ListView listView = (ListView) findViewById(R.id.listview);
		listView.setAdapter(adapter);

	Didalam Main_Activity akan terlihat seperti ini:

		public class MainActivity extends Activity {
	    String[] listArray={"Asus","Acer","Apple","Samsung","Thoshiba","Sony","Xiomi","Motorola"};
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	        ArrayAdapter adapter = new ArrayAdapter(this,R.layout.activity_listview,listArray);
	        ListView listview =(ListView) findViewById(R.id.array_list);
	        listview.setAdapter(adapter);
	    	}
		}
	Lalu tambahkan ListView dan TextView di file xmlnya.

2. FRAGMENT
	Fragment adalah salah satu komponen, antar muka (user interface ) yang merupakan sebuah bagian dari Activity, dapat disebut juga dengan nama Sub-Activity. Satu Activity bisa mengelola beberapa fragment. untuk menampilkan hasil di layar user (pengguna). Dalam Satu Activity juga, sebuah fragment dapat diganti, ditambahkan dan dihapus dan juga bersifat reusable, artinya dapat digunakan kembali sesuai kebutuhan. Fragment dipengaruhi dari lifecycle (siklus hidup ) Activity, karna Fragment termasuk bagian dari Activity.
	Saat membuat fragment alangkah baiknya jika kita membuat folder baru khusus fragment supaya lebih rapi.

			onAttach(Activity) : digunakan untuk memanggil 1 kali ketika menempel di Activity.
			onCreate(Bundle) : digunakan untuk mempersiapkan fragment.
			onCreateView(LayoutInflater, ViewGroup, Bundle) : menciptakan dan menampilkan kembali secara hirarki View.
			onActivityCreated(Bundle) : method ini dipanggil setelah method onCreate().
			onViewStateRestored(Bundle) : digunakan untuk menyatakan informasi  kepada fragment bahwa semua akan disimpan ke dalam state (layar) dari tampilan fragment secara hirarki yang telah dipulihkan.
			onStart() : digunakan untuk membuat fragment terlihat.
			onResume() : digunakan untuk membuat fragment interaktif.
			onPause() : digunakan jika fragment tidak lagi interaktif.
			onStop() :digunakan jika fragment tidak lagi  terlihat.
			onDestroyView() : digunakan untuk membersihkan resources (sumber daya.
			onDestroy() : digunakan untuk membersihkan akhir resources (sumber daya )dari layar fragment.
			onDetach() : digunakan ketika fragment ,tidak lagi ada di Activity.
	Untuk 

3. SQLITE DATABASE