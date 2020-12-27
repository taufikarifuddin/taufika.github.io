# UNIT TEST

Menurut pengertian yang di kutip dari [wikipedia](https://en.wikipedia.org), [Unit Test](https://en.wikipedia.org/wiki/Unit_testing) merupakan salah satu metode yang digunakan untuk melakukan test pada source code yang telah dibuat. Dari kata-kata penyusun Unit Test sendiri terdiri dari 2 kata, **UNIT** dan **TEST**, tidak lain yang berarti adalah melakukan **TEST** pada sebuah **UNIT**. Jadi dapat disimpulkan, unit test ini hanya melakukan test dalam scope unit, bukan secara flow secara menyeluruh.

# Apa yang akan di bahas ? 
Ada beberapa point yang akan dibahas dalam tulisan ini, diantaranya adalah :

1. [Mengapa Tertarik membahas unit test ?](#Alasan-Membehas-Unit-Test)
2. [Kenapa dan ada apa dengan unit test ?](#Kenapa-dan-ada-apa-dengan-unit-test)
3. [Kenapa developer yang melakukannya ?](#Kenapa-developer-yang-melakukannya-)
4. [Seberapakah pentingnya ? ](#Seberapa-pentingnya)
5. [Bagaimana implementasinya ?](#Bagaimana-implementasinya)
6. [Kekurangan](#Kekurangan)
7. [Summary](#Summary)


## Alasan Membehas Unit Test
Alasan yang kuat mengapa tertarik membahas mengenai unit test berawal dari pengalaman pribadi yang bisa dibilang merupakan [Culture Shock](https://en.wikipedia.org/wiki/Culture_shock) bagi saya secara pribadi. Hal tersebut dikarenakan harus menulis unit test dengan syarat tingkat line coverage yang cukup tinggi untuk seorang yang baru mengenal Unit Test. Selain itu, Unit Test sendiri sangat penting untuk menjaga sebuah kualitas code ketika membuat sebuah aplikasi dan bahkan beberapa perusahaan menjadikannya syarat ketika proses development.

Jadi hal tersebutlah yang mendorong menulis artikel ini. Penulis berharap agar pembaca dapat mengerti point apa dan bagaimana unit test tersebut dilakukan, sebarapa penting unit test ini untuk diimplementasikan.

## Kenapa dan ada apa dengan unit test
Mungkin beberapa developer bertanya-tanya kenapa harus **Unit Test** atau bahkan tidak tau tentang **Unit Test** itu sendiri. Mari kita bahas apa itu **Unit Test** dan apa issue yang berkaitan dengan Unit Test.

Seperti yang sudah dijelaskan [diatas](#UNIT-TEST), **Unit Test** merupakan salah satu cara yang digunakan untuk melakukan test terhadap code yang telah kita buat, apakah sudah memenuhi ekspektasi dengan apa yang kita inginkan.

Apakah Harus Dengan **Unit Test** ? tentu saja tidak, banyak beberapa cara untuk melakukan hal tersebut, kita bisa menggunakan [Integration Test](https://en.wikipedia.org/wiki/Integration_testing) untuk scope yang lebih luas atau **"Test Test"** yang lain. Bahkan mungkin masih terdapat beberapa developer yang melakukan test dengan meng-run aplikasi dan mencobanya secara manual.

Apakah itu salah ? menurut saya, TIDAK, kenapa ? karena pada dasarnya itu merupakan salah satu cara yang dilakukan beberapa developer untuk melakukan test untuk memastikan source code yang telah ditulis sesuai dengan ekspektasi. Yap, kenapa saya mengatakan itu tidak salah, karena cara tersebut adalah cara yang saya gunakan sebelum mengetahui Unit Test.

Kendala yang umum kenapa orang tidak mengimplementasikan **Unit Test** ini selain tidak mengetahui **Unit Test** itu sendiri tidak lain adalah **WAKTU**. Kenapa ? ketika membuat **Unit Test**, tentu saja akan memakan waktu dari developer. Developer harus menulis test terhadap source code yang telah kita buat, bahkan seringkali kita harus mempersiapkan test dengan banyak case untuk 1 komponen yang memang itu memiliki logika yang rumit. So, kenapa developer harus  menghabiskan waktu untuk menulis code dan harus membuat **Unit Test** yang memakan waktu juga ? bukankah itu akan menghambat **"timeline"** dari proses development ?

YES, tentu saja hal tersebut akan menghambat proses deveopment **"JIKA tidak dialokasikan sebagai proses development juga"**. Hal tersebutlah yang menjadi kendala yang umum mengapa beberapa developer mungkin "enggan" atau "tidak setuju" untuk melakukan **Unit Test** apalagi ditambah untuk menulis **Unit Test** dengan test case yang bermacam-macam. Dan itu juga yang menjadi beban pikiran saya saat melakukan transisi yang mewajibkan **Unit Test** sebagai syarat wajib untuk melakukan menggabungkan code yang telah saya tulis.

Lalu apa yang saya akan dapatkan setelah mengimplementasikan **Unit Test** ? [let's move to another topic](#Seberapa-pentingnya)

## Kenapa developer yang melakukannya
Mungkin ini yang salah satu jadi pertanyaan, mengapa Developer yang melakukannya ?  Haruskah developer yang melakukannya ? kenapa bukan QA (Quality Assurance) ?

Menurut pendapat saya pribadi, **YES**. As Developer, kita harus memastikan semua produk yang telah dideliver ke customer harus sudah sesuai dengan requirement yang telah ditetapkan termasuk terdapat case case yang memang harus dihandle.

Kenapa bukan QA (Quality Assurance) yang melakukan test-test tersebut ? Well, sebenarnya menurut saya pribadi, QA memiliki scope pengetesan yang lebih luas dari developer. So, jika QA harus melakukan test yang seharusnya itu harus harus sesuai dengan requirement, kapan QA akan melakukan test terhadap **"unexpected case"** ???

## Seberapa pentingnya
Seberapa pentingkah **Unit Test** ? Lalu apa yang saya akan dapatkan setelah mengimplementasikan **Unit Test** setelah mengorbankan waktu untuk melakukannya ?

Coba bayangkan, ketika developer membangun sebuah sistem yang memiliki flow approval, dan kita akan melakukan test terhadap report yang di generate ketika flow approval sudah terpenuhi. Bagaimana flow melakukan test secara manual ? Dalam pengalaman saya, tentu saja akan melakukan approval flow dan generate, jika terdapat bug / sesuatu yang tidak sesuai dengan ekspektasi, akan di fix dan akan dicoba ulang, secara flow bisa di gambar sebagai berikut : 
```
              No                        No                        No
run app ----|----> doing approval flow | -----> generate report |---> successfully generate
    ^       |bug?                      |bug ?                   |bug ?
    |       |      yes                 |                        |
fix code <--|--------------------------|------------------------|
                                                    
```
dari ilustrasi di atas, flow approval developer akan melakukan test secara manual, dan akan melakukan perulangan untuk melakukan test yang sama ketika terdapat kesalahan dalam proses tersebut. 

Lalu apa yang kita lakukan di Unit Test ?
```                          yes
write/fix unit test and codes --------> run test----|---> success 
        ^                                           |
        |                                           | as expected ?
evaluate the code / unit test-----------------------|
                                no
```
Ilustrasi diatas yang akan di lakukan dengan **Unit Test**, developer melakukan proses tersebut untuk approval flow dan generate report. Dan keuntungannya adalah developer dapat melakukan verify yang telah dibuat, fix bug dan melakukan verify ulang dan  membuat banyak test case untuk masing-masing proses tersebut tanpa harus mengulang proses dari awal dan dilakukan secara automate berdasarkan Unit Test yang telah ditulis. Seperti contohnya, untuk test case yang bisa kita test untuk proses approval :
- user selain role yang berhak, tidak dapat melakukan approval
- user tidak dapat melakukan generate report jika approvalnya tidak terpenuhi
- user yang melakukan approval harus ada dan valid

dan masih banyak lagi test case yang developer bisa masukkan tanpa harus melakukannya secara manual, dan langsung mengetahui jika ada kesalahan logic ketika test case yang developer masukkan tidak sesuai dengan hasil test case yang di lakukan.

Ada beberapa keuntungan lain selain diatas, seperti ketika developer menemukan bug di flow tertentu, developer cukup menambahkan test sesuai dengan case yang menyebabkan bug, dan run unit test, untuk melakukan verify tanpa harus melakukannya dari awal. Dengan begitu developer dapat memastikan bug yang telah terjadi sudah dihandle dan tidak mengganggu flow dari logika yang sebelumnya sudah valid.

Selain beberapa keuntungan yang telah disampaikan diatas, beberapa startup besar juga sudah menerapkan dan mewajibkan unit test sebagai salah satu cara untuk melakukan verify terhadap apa yang kita buat. 


## Bagaimana implementasinya ?

Here we go !!, seteah membicarakan mengenai **Unit Test** itu sendiri, terus bagaimana untuk  implementasinya ?. So, sebelum mulai untuk melakukan implementasinya, kita mulai dari kata **"Unit"** di **Unit Test** itu sendiri. Perlu di garis bawahi **Unit** disini berarti individu, jadi disini scopenya hanya indvidu tersebut yang berarti tidak memikirkan dependency lain yang memanggil ataupun dipanggil oleh si **Unit** ini, bagaimana maksudnya ? jika kita memiliki 2 class yang saling berkaitan, misal Class A memanggil Class B yang merupakan Helper, untuk membuat **Unit Test** dari class B, kita dapat menghiraukan bagaimana Class A ini memanggil class B. Kita hanya fokus terhadap komponen-komponen yang di test pada class B. Untuk lebih jelasnya, 

### **let's go deep into the implementation :**
Sebelumnya saya menggunakan Java dan JUnit untuk melakukan **Unit Test**

#### Simple Unit Test
```
public class Utility {
    public static Long denullifyAmount(Long amount){
        return Optional.ofNullable(amount)
                .orElse(0l);
    }

    public static Long sumAmounts(Long ...amounts){
        long totalAmount = 0;
        for (Long amount: amounts) {
            totalAmount += denullifyAmount(amount);
        }
        return totalAmount;
    }
}
```
diatas sudah terdapat class Utitlity yang memiliki 2 method. 
1. methdo denullify amount digunakan untuk menghandle jika ```amount == null``` maka akan mengembalikan 0 agar tidak terjadi NPE ketika di proses lebih lanjut.
2. digunakan untuk melakukan penjumlahan terhadap beberapa amount

So mari kita buat unit testnya : 
yang pertama mari kita buat untuk unit test untuk ke 2 method tersebut :
```
public class UtilityTest {

    @Test
    public void denullifyAmount_NullAmountTest(){
        Long actualResult = Utility.denullifyAmount(null);
        assertEquals(Long.valueOf(0l), actualResult);
    }

    @Test
    public void denullifyAmountSuccessTest(){
        Long actualResult = Utility.denullifyAmount(10l);
        assertEquals(Long.valueOf(10l), actualResult);
    }

    @Test
    public void sumAmmountSuccessTest(){
        Long actualResult = Utility.sumAmounts(10l, 10l, 1l);
        assertEquals(Long.valueOf(21l), actualResult);
    }

    @Test
    public void sumAmmount_nullAmountTest(){
        Long actualResult = Utility.sumAmounts(10l, null, 1l);
        assertEquals(Long.valueOf(11l), actualResult);
    }

    @Test
    public void sumAmmount_allNullAmountTest(){
        Long actualResult = Utility.sumAmounts(null, null, null);
        assertEquals(Long.valueOf(0l), actualResult);
    }
}
```
ketika ada case, ternyata amount tidak boleh bilangan negative, sehingga kita harus sedikit mengubah logic pada ```denullifyAmount(Long amount)``` menjadi : 
```
public static Long denullifyAmount(Long amount){
        if(amount != null && amount < 0){
            throw new IllegalArgumentException("amount should be positive number");
        }
        return Optional.ofNullable(amount)
                .orElse(0l);
    }
```
So, bagaimana dengan unit testnya ? nah, ini salah satu keuntungan yang ditawarkan oleh **Unit Test**. Pada dasarnya, perubahan tersebut seharusnya tidak ada hubungannya dengan logika logika yang telah dibuat sebelumnya. Jadi seharusnya untuk test sebelumnya tidak ada perubahan sama sekali, kita hanya perlu menambahkan **Unit Test** ketika amountnya adalah negative. Sehingga testnya akan menjadi seperti dibawah : 
```
public class UtilityTest {

    @Test
    public void denullifyAmount_NullAmountTest(){
        Long actualResult = Utility.denullifyAmount(null);
        assertEquals(Long.valueOf(0l), actualResult);
    }

    @Test
    public void denullifyAmountSuccessTest(){
        Long actualResult = Utility.denullifyAmount(10l);
        assertEquals(Long.valueOf(10l), actualResult);
    }

    @Test
    public void sumAmmountSuccessTest(){
        Long actualResult = Utility.sumAmounts(10l, 10l, 1l);
        assertEquals(Long.valueOf(21l), actualResult);
    }

    @Test
    public void sumAmmount_nullAmountTest(){
        Long actualResult = Utility.sumAmounts(10l, null, 1l);
        assertEquals(Long.valueOf(11l), actualResult);
    }

    @Test
    public void sumAmmount_allNullAmountTest(){
        Long actualResult = Utility.sumAmounts(null, null, null);
        assertEquals(Long.valueOf(0l), actualResult);
    }

    @Test(expected = IllegalArgumentException.class)
    public void denullifyAmount_negativeAmount(){
        Utility.denullifyAmount(-1l);
    }

    @Test(expected = IllegalArgumentException.class)
    public void sumAmmount_negativeAmountTest(){
        Utility.sumAmounts(10l, -1l, 10l);
    }

    @Test(expected = IllegalArgumentException.class)
    public void sumAmmount_nullAndNegativeTest(){
        Utility.sumAmounts(null, null, -1l);
    }
}
```
Jika diperhatikan, tidak ada perubahan pada test sebelumnya. Hanya ada penambahan test untuk case terbaru yaitu ```denullifyAmount_negativeAmount()```, ```sumAmmount_negativeAmountTest()``` dan ```sumAmmount_nullAndNegativeTest()```. Ketika kita run tersebut dan terdapat error di test sebelumnya, maka perlu ada pengecekan karena terdapat indikasi bahwa perubahan code yang baru tersebut mempengaruhui flow yang sebelumnya.

## Kekurangan
Sebelumnya sudah dijelaskan, apa itu ``Unit Test`` dan pengertiannya. Unit test sendiri adalah salah satu metode yang digunakan melakukan pengetesan di level ``Unit``. Jadi kita tidak perlu memikirkan bagaimana si ``Unit`` ini dipanggil oleh ``Unit`` lain. Sehingga kita tidak dapat memastikan proses integrasi antara class sudah berjalan dengan benar atau tidak. Seperti contohnya case yang sering terjadi adalah parameter yang dikirim  dari ``Unit`` yang akan di test ternyata salah dari sisi ``Unit`` pemanggil. Karena hal tersebut ada beberapa metode lain seperti ``Integration Test`` yang dapat dikombinasikan untuk bisa mengcover hal tersebut.

Selain kasus diatas, hal yang mungkin menjadi perhatian adalah waktu dalam proses yang menjadi semakin panjang karena harus membuat Unit Test untuk code yang telah dibuat. 

## Summary
**Unit Test** merupakan salah satu metode yang digunakan untuk melakukan test pada source code yang telah dibuat. **Unit Test** sendiri banyak memberikan keuntungan dalam pengimplementasiannya seperti dapat mengurangi effort untuk melakukan test terhadap source code yang akan dibuat. Banyak tools-tools yang memudahkan untuk menulis Unit Test seperti [JUnit](https://junit.org/) JUnit untuk java, [PhpUnit](https://phpunit.de/) untuk PHP, dan banyak lagi.
