# STM32 Harici Buton ile Led Uygulaması 
Bu uygulamada STM32F042K6T6 Geliştirme Kartına harici olarak bağlanan bir buton ile kartın üzerindeki led kontrol edildi. 
## Donanım
Kartın üzerinde bir adet Reset butonu bulunurken kullanıcının kontrol edebileceği başka herhangi bir buton mevcut değildir. Bizde harici olarak Pull-Down direnci kullanarak bir buton yapısı oluşturup oradan elde edeceğimiz sinyali kartımızın üzerinde bulunan D6(PB_1) pinine giriş olacak şekilde bağlayacağız. Bu şekilde butona bastığımız veya basmadığımız durumları bu pin ile okuyup, durumumuza göre ledimizi kontrol edeceğiz.

![alt text](https://github.com/RuchanKURT/STM32-Buton-ile-Led/blob/main/pull%20down%20resistor.png)
Gerekli bağlantıları kartımıza yaptıktan sonra STM32 CUBEIDE programı üzerinden projemizi oluşturup PB3(Led) pinimizi *GPIO_OUTPUT* ve PB1(Buton) pinimizi *GPIO_INPUT* olarak ayarlayıp kaydediyoruz. *int main()* fonksiyonumuzun üst kısmına ledin açık/kapalı olma durumuna göre bir enum yapısı tanımladık ve bu durumları open/close şeklinde yazdık. Program close = 0 ve open = 1 olarak algılayacaktır. Bunun altında ise C dilinde kolaylık açısından sıkça kullanılan bir struct yapımız var. Programın içinde bulunan birbiriyle ilişkili birden fazla değişkenin tek çatı altında toplanmasıdır aslında ve bu çokça kolaylık sağlar. Butonumuz Port adını, Pin adını, o anki durumunu ve ledimizin durumunu tanımladığımız bir struct yapısı oluşturduk ve bu genel yapıya buton1 ismini verdik. int main() fonksiyonumuz içindeki while(1) döngümüzün içine kodlarımızı yazdık (Resim-4). Burada program anlık olarak butonun durumunu okuyor ve butona bastığımız an Resim-5’te görüldüğü gibi sinyal Pozitif Düzeye geçiyor. Ne zamanki kullanıcı elini butondan çekerse yani sinyal Düşen Kenar durumuna geçerse ledimiz yanıyor. Bu işlemlerin yanında struct yapımızın içindeki değişkenlerimizde o anki duruma göre if-else komutları sayesinde değişiklik gösteriyor. Program bu şekilde hem buton ile kart üzerinde bulunan ledi kontrol etmemizi hem de bu durumların karşılığını anlık olarak CUBEIDE programı üzerinden okuyabilmemizi sağlıyor. 
