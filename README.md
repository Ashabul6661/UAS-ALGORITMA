Narasi Proses Analisis Data IPM Indonesia (2022–2024)
Dalam proyek ini, dilakukan analisis terhadap data Indeks Pembangunan Manusia (IPM) berdasarkan provinsi di Indonesia untuk tahun 2022 hingga 2024. Data diambil dari situs resmi Badan Pusat Statistik (BPS) dan dianalisis menggunakan Python dengan memanfaatkan beberapa library dan framework sebagai berikut:
Framework dan Library yang Digunakan:
1). Streamlit dalam  #Load data
Framework ini digunakan sebagai web-based interface untuk menampilkan hasil analisis secara interaktif di localhost. Streamlit memungkinkan pembuatan dashboard visualisasi yang sederhana namun powerful tanpa memerlukan pengetahuan mendalam tentang front-end development. 
Langkah pertama dalam analisis ini adalah memuat data IPM dari file CSV menggunakan pd.read_csv(), yang mengubah data tabular menjadi DataFrame agar mudah diolah. 
Setelah data berhasil dimuat, dilakukan pembersihan nama kolom menggunakan strip() untuk menghapus spasi yang mungkin tersembunyi, guna mencegah kesalahan saat pemanggilan kolom.
Selanjutnya, isi kolom 'Provinsi' juga dibersihkan dari spasi awal dan akhir menggunakan str.strip(), agar nama provinsi konsisten dan siap digunakan dalam analisis lebih lanjut seperti pencocokan data atau visualisasi peta.
2). JSON pada #Load geojson
Format data ini digunakan untuk memuat data peta (geojson) wilayah Indonesia, yang kemudian dikombinasikan dengan data IPM untuk ditampilkan pada peta interaktif menggunakan Streamlit Folium.
Pertama, dengan menggunakan perintah with open(...) as geo, file indonesia-prov.geojson dibuka dalam mode baca ("r") dengan encoding UTF-8 untuk memastikan karakter dibaca dengan benar. 
Selanjutnya, isi file dibaca dan diubah menjadi struktur data Python menggunakan json.load(geo), lalu disimpan dalam variabel geojson_data. 
3). Pandas pada #Statistik
Digunakan sebagai library utama untuk memuat, membersihkan, dan menganalisis data tabular. Data IPM dari tahun 2022 hingga 2024 yang diperoleh dari BPS diolah menggunakan pandas untuk mendapatkan informasi statistik deskriptif serta melakukan transformasi data.
Pertama, avg_ipm = data_tahun['IPM'].mean() menghitung rata-rata IPM dari seluruh provinsi pada tahun tersebut. 
Kemudian, max_row = data_tahun.loc[data_tahun['IPM'].idxmax()] digunakan untuk mengambil baris data dengan nilai IPM tertinggi, yaitu provinsi dengan IPM paling tinggi. 
Sebaliknya, min_row = data_tahun.loc[data_tahun['IPM'].idxmin()] mengambil baris dengan nilai IPM terendah, yaitu provinsi dengan IPM paling rendah.
4). Streamlit, Pandas dan Plotly pada #Radar chart
Streamlit merupakan framework ini digunakan sebagai web-based interface untuk menampilkan hasil analisis secara interaktif di localhost. Streamlit memungkinkan pembuatan dashboard visualisasi yang sederhana namun powerful tanpa memerlukan pengetahuan mendalam tentang front-end development.
Pandas digunakan sebagai library utama untuk memuat, membersihkan, dan menganalisis data tabular. Data IPM dari tahun 2022 hingga 2024 yang diperoleh dari BPS diolah menggunakan pandas untuk mendapatkan informasi statistik deskriptif serta melakukan transformasi data.
Plotly untuk memperkuat pemahaman terhadap variasi antar provinsi, radar chart dibuat untuk tahun tertentu (misalnya 2024), menampilkan perbandingan IPM seluruh provinsi dalam satu visualisasi.
Pertama, st.subheader("Radar Chart IPM per Provinsi") menambahkan subjudul pada tampilan aplikasi Streamlit. 
Selanjutnya, radar_data = data_tahun.sort_values("IPM", ascending=False).head(5) menyortir data berdasarkan nilai IPM dari yang tertinggi, lalu mengambil 5 provinsi teratas. 
Kemudian, px.line_polar(...) dari library Plotly Express digunakan untuk membuat radar chart, dengan sumbu radial (r) mewakili nilai IPM dan sumbu sudut (theta) mewakili nama provinsi. Parameter line_close=True digunakan untuk menutup garis radar, 
Sedangkan markers=True menambahkan titik pada tiap sudut. Grafik ini bertujuan memberikan gambaran visual yang jelas mengenai perbandingan IPM antar provinsi terbaik, dan ditampilkan dalam dashboard Streamlit menggunakan st.plotly_chart(fig_radar). 
5). Streamlit Folium pada Peta
Digunakan untuk menampilkan peta interaktif yang menggambarkan persebaran nilai IPM per provinsi di Indonesia. Data koordinat provinsi dimuat dalam format geojson, lalu dihubungkan dengan nilai IPM untuk masing-masing provinsi guna menghasilkan peta tematik (choropleth map) yang menarik dan informatif.
Pertama, baris st.subheader("Peta Interaktif IPM") menampilkan subjudul pada aplikasi Streamlit sebagai penanda bahwa bagian ini berisi peta. 
Selanjutnya, objek m dibuat dengan menggunakan folium.Map(), yang merupakan bagian dari library folium untuk visualisasi peta berbasis Leaflet.js. Parameter location=[-2.5, 118] menentukan titik pusat peta, yaitu koordinat geografis Indonesia, sementara zoom_start=5 mengatur tingkat zoom agar seluruh wilayah Indonesia terlihat. 
Terakhir, tiles="CartoDB positron" digunakan untuk memilih gaya tampilan peta yang bersih dan modern, cocok untuk overlay data tematik seperti nilai IPM per provinsi. Peta ini nantinya akan dilengkapi dengan layer data IPM untuk menampilkan perbedaan visual antar wilayah.
6). JSON dan Pandas
JSON merupakan format data ini digunakan untuk memuat data peta (geojson) wilayah Indonesia, yang kemudian dikombinasikan dengan data IPM untuk ditampilkan pada peta interaktif menggunakan Streamlit Folium.
Pandas digunakan sebagai library utama untuk memuat, membersihkan, dan menganalisis data tabular. Data IPM dari tahun 2022 hingga 2024 yang diperoleh dari BPS diolah menggunakan pandas untuk mendapatkan informasi statistik deskriptif serta melakukan transformasi data.
Proses diawali dengan melakukan iterasi pada setiap fitur (provinsi) dalam geojson_data['features']. Pada setiap iterasi, nama provinsi diambil dari properti 'Provinsi' dan dibersihkan dari spasi dengan strip(). 
Kemudian, data IPM untuk provinsi tersebut dicari dalam data_tahun dengan mencocokkan nama provinsi. Jika nilai IPM ditemukan (not nilai.empty), maka nilai tersebut dikonversi ke bentuk float dan ditambahkan ke properti GeoJSON menggunakan feature['properties']['IPM']. Jika tidak ditemukan, maka nilai IPM diatur menjadi None.
