# project-shell-tugas4
Project ini dibuat untuk memenuhi tugas 4 Shell tooling dari Pacmann.ai

# Detail task
1. Langkah pertama yang dilakukan adalah mendownload file dari Google Drive dan memindahkannya ke dalam folder tugas 4 ini
2. Menggabungkan 2 file 2019-Oct-sample.csv dan 2019-Nov-sample.csv menjadi file Oct_Nov.csv dengan: csvstack 2019-Oct-sample.csv 2019-Nov-sample.csv >> Oct_Nov.csv
3. Mem-filter kolom event_type agar hanya menampilkan "purchase" dan memfilter kolom yang akan ditampilkan(event_time,event_type,product_id,category_is,brand,price,category_code)
: csvgrep -c "event_type" -m "purchase" Oct_Nov.csv | csvcut -c 2-5,7-8,6 > purchase_raw.csv
4. Memisahkan kolom category_code untuk dipisahkan menjadi category saja (splitting strings)
: csvgrep -c "event_type" -m "purchase" Oct_Nov.csv | awk -F "." '{print $1}' | csvcut -c "category_code" > purchase_category.csv
Note: Saya memisahkan dulu kolom ini karena jika tidak dipisahkan kolom "price" ikut ter-split
5. Sata mengulang langkah ke-4 untuk mengambil kolom yang akan menjadi product_name
: csvgrep -c "event_type" -m "purchase" Oct_Nov.csv | csvcut -c 2-5,7-8,6 > purchase_raw.csv
6. Split string dari category_code menampilkan kolom product_code 
: cat purchase_data_coba.csv | awk -F "." '{print $NF}' | csvcut -c 1 > purchase_product_name.csv
7. Mem-filter kolom (event_time,event_type,product_id,category_is,brand,price), membuang kolom category_code
: csvgrep -c "event_type" -m "purchase" Oct_Nov.csv | csvcut -c 2-5,7-8 > purchase_filter.csv
8. Menggabungkan kolom yang sudah ter-filter dengan kolom yang sudah di-split
: paste -d , purchase_clean.csv purchase_category.csv purchase_product_name.csv > purchase_data_cleaning.csv
9. Mengubah nama kolom category_code menjadi category
: sed -i 's/category_code/category/' purchase_data_cleaning.csv

# Saran perbaikan
1. Untuk nama kolom yang seharusnya product_code masih bernama event_time entah kenapa tidak bisa diganti
2. Saran untuk mendapatkan cara yang lebih efektif untuk split strings dalam category code

Terima kasih


