# STM32 Harici Buton ile Led Uygulaması 
Bu uygulamada STM32F042K6T6 Geliştirme Kartına harici olarak bağlanan bir buton ile kartın üzerindeki led kontrol edildi. 
## Donanım
Kartın üzerinde bir adet Reset butonu bulunurken kullanıcının kontrol edebileceği başka herhangi bir buton mevcut değildir. Bizde harici Pull-Down direnci kullanarak bir buton yapısı oluşturup oradan elde edeceğimiz sinyali kartımızın üzerinde bulunan D6(PB_1) pinine giriş olacak şekilde bağlayacağız. Pull-Down direnç yapısı ile bağladığımız zaman butona basmadığımızda D6 pinimiz '0' değerini okuyacak, butona bastığımız süre içerisinde ise pinimiz '1' değerini okuyacak. Bu şekilde butona bastığımız veya basmadığımız durumları bu pin ile okuyup, durumumuza göre ledimizi kontrol edeceğiz.

![alt text](https://github.com/RuchanKURT/STM32-Buton-ile-Led/blob/main/pull%20down%20resistor.png)
## Yazılım
Gerekli bağlantıları kartımıza yaptıktan sonra STM32 CUBEIDE programı üzerinden projemizi oluşturup PB3(Led) pinimizi *GPIO_OUTPUT* ve PB1(Buton) pinimizi *GPIO_INPUT* olarak ayarlayıp kaydediyoruz. Kodumuzun üstündeki *Include* kısmına ek olarak kullanacağımız *strcpy* gibi bazı komutlardan dolayı *<string.h>* kütüphanesini ekliyoruz. *int main()* ana fonksiyonumuzun dışına, ledin açık/kapalı olma durumuna göre bir enum yapısı tanımladık ve bu durumları open/close(1/0) şeklinde yazdık. Bunun altında ise C dilinde kolaylık açısından sıkça kullanılan bir struct yapısı tanımladık. Bu yapı programın içinde bulunan birbiriyle ilişkili birden fazla değişkenin tek çatı altında toplanmasıdır aslında. Butonumuzun Port adını, Pin adını, o anki durumunu ve ledimizin durumunu tanımladığımız bir struct yapısı oluşturduk ve bu genel yapıya *buton1* ismini verdik.
```C
/* USER CODE BEGIN PFP */
typedef enum{
	close,
	open
}state_t;

typedef struct{
	char buton_PortAdi[20];
	char buton_PinAdi[20];
	state_t buton_durumu;
	state_t led_durumu;
}buton;
buton buton1;
/* USER CODE END PFP */
```
*int main()* fonksiyonumuz içindeki *while(1)* döngümüzde program anlık olarak butonun durumunu okuyor ve butona bastığımız an Resimde görüldüğü gibi sinyal Pozitif Düzeye geçiyor. Ne zamanki kullanıcı elini butondan çekerse yani sinyal Düşen Kenar durumunda olursa ledimiz yanıyor. Yani kullanıcı butona anlık olarak basıp elini çektiği zaman aslında sistem bastığı an değil, elini çektiği an içine yazdığımız kodları uyguluyor. Bu şekilde yapmamızın nedeni butona basarken meydana gelebilecek *debounce* etkisini en aza indirgememiz.

![alt text](https://github.com/RuchanKURT/STM32-Buton-ile-Led/blob/main/düşen_yükselen_Kenar.png)
```C
if(HAL_GPIO_ReadPin(Buton_GPIO_Port, Buton_Pin))
{
	HAL_Delay(15);
	if(HAL_GPIO_ReadPin(Buton_GPIO_Port, Buton_Pin) == 0)
	{
		HAL_GPIO_TogglePin(Led_GPIO_Port, Led_Pin);
		HAL_Delay(300);
	}
}
```
Bu işlemlerin yanında struct yapımızın içindeki değişkenlerimizde o anki duruma göre if-else komutları sayesinde değişiklik gösteriyor. Program bu şekilde hem buton ile kart üzerinde bulunan ledi kontrol etmemizi hem de bu durumların karşılığını anlık olarak CUBEIDE programı üzerinden okuyabilmemizi sağlıyor.
