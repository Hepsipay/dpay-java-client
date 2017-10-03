# dpay-java-client

# Kullanım

### 3D’siz Satış İşlemi

#### Request

```Java
package com.hepsipay.sample;

import java.util.ArrayList;
import java.util.List;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

import com.hepsipay.client.model.Basketitem;
import com.hepsipay.client.model.Card;
import com.hepsipay.client.model.Customer;
import com.hepsipay.client.model.Extra;
import com.hepsipay.client.model.InvoiceAddress;
import com.hepsipay.client.model.ShippingAddress;
import com.hepsipay.client.model.request.payment.Payment;
import com.hepsipay.client.model.response.PaymentResponse;
import com.hepsipay.client.service.PaymentService;
import com.hepsipay.client.type.BasketItemType;
import com.hepsipay.client.type.Currency;
import com.hepsipay.client.util.HepsiPayUtil;

/**
 * 
 * @author Ahmet Faruk Bişkinler,
 * @Support PixelTürk Web Studio, "destek@pixelturk.net"
 *
 */
public class SamplePayment extends Sample {
	private final static Logger logger = LogManager.getRootLogger();
	
	public static void main(String[] args) {
		Payment paymentRequest = new Payment();
		
		/**
		 * Create Payment Request
		 * Ödeme İsteği Oluştur
		 */
		//		paymentRequest.setTransactiontime();    // Ödeme İşlem Zamanı, Varsayılan zaman ise şuan.
		//		paymentRequest.setTransactionId("tx_" .  rand(100000, 999999));    // Ödemeye Ait Tekil Kodu (Maksimum 40 karakterdir)
		paymentRequest.setTransactionId(HepsiPayUtil.generateRandomTranstionId()); // Ödemeye Ait Tekil Kodu (Maksimum 40 karakterdir)
		paymentRequest.setDescription("E-Ticaret Ödemesi"); // Açıklama alanıdır (Maksimum 40 karakterdir)
		paymentRequest.setAmount("120,99"); // Toplam Ödeme Tutarı
		paymentRequest.setCurrency(Currency.TL.getCode()); // Toplam Ödeme Tutar Birimi
		paymentRequest.setInstallment(1); // Ödeme Taksit Sayısı (1'den 12'ye kadar taksit değeridir)
		
		/**
		 * Credit Card Info
		 * Kredi Kartı Bilgileri
		 */
		Card card = new Card();
		card.setCardHolderName("John Doe"); // Kart İsim Soyisim (Maksimum 40 karakterdir. Sadece alfabetik karakterler ve boşluk kabul eder)
		card.setCardNumber("4355084355084358"); // Kart Numarası (15 veya 16 haneli nümerik değerdir)
		card.setExpireMonth("12"); // Kart Son Kullanım Tarihi (2 haneli nümerik değerdir.)
		card.setExpireYear("18"); // Kart Son Kullanım Tarihi (2 haneli nümerik değerdir.)
		card.setSecuritycode("000"); // Kart CVV (3 veya 4 haneli nümerik değerdir.)
		paymentRequest.setCard(card);
		
		/**
		 * Customer Info
		 * Müşteri Bilgileri
		 */
		Customer customer = new Customer();
		customer.setName("Ali"); // Müşteri İsim (Maksimum 20 karakterdir. Sadece alfabetik karakterler ve boşluk kabul eder.)
		customer.setSurname("Veli"); // Müşteri Soyisim (Maksimum 20 karakterdir. Sadece alfabetik karakterler ve boşluk kabul eder.)
		customer.setEmail("ali.veli@alivelimarket.com.tr"); // Müşteri Mail (Eposta adresidir.)
		customer.setIpaddress("72.125.165.16"); // Müşteri IP Adresi (IP adresidir.)
		customer.setPhonenumber("+905350000000"); // Müşteri Telefon Numarası (Maksimum 13 karakterdir. Sadece nümerik ve + değerlerini alabilir.)
		customer.setCode("7cefdf61-38cd-4b35-b5f0-4c98c5805d41"); // Müşteri Kodu (Maksimum 36 karakterdir.)
		customer.setTckn("12345678910"); // Müşteri Kimik Numarısı (TC Kimlik numarasıdır.)
		customer.setVatNumber("12345678910"); // Müşteri Vergi Numarası (Şirketlere ve firmalara tekil vergi numarası)
		customer.setMemberSince("20151124"); // Müşteri Üyelik tarihi (Müşteri üyelik tarihi formatı YYYYMMDD’dır.)
		customer.setBirthdate("19781123"); // Müşteri Doğum tarihi (Müşteri üyelik tarihi formatı YYYYMMDD’dır.)
		paymentRequest.setCustomer(customer);
		
		/**
		 * Shipping Address
		 * Teslimat/Nakliye Adresi
		 */
		ShippingAddress shippingAddress = new ShippingAddress();
		shippingAddress.setName("Ali Veli"); // Sipariş Teslimatının yapılacağı kişi adı ve soyadı (Maksimum 40 karakterdir. Sadece alfabetik karakterler ve boşluk kabul eder.)
		shippingAddress.setAddress("Kuştepe Mahallesi Mecidiyeköy Yolu Cad. No:12 Trump Towers Kule:2 Kat:11 Mecidiyeköy"); // Siparişin teslim edileceği adres (Maksimum 500 karakterdir. Sadece alfanümerik karakterler, &.,/: ve boşluk karakterini kabul eder.)
		shippingAddress.setCountry("Türkiye"); // Sipariş teslimatının yapılacağı ülke (Maksimum 20 karakterdir. Sadece alfabetik olabilir.)
		shippingAddress.setCountrycode("TUR"); // Sipariş teslimatının yapılacağı ülke (2 veya 3 haneli ISO ülke kodudur.Sadece alfabetik olabilir.)
		shippingAddress.setCity("Istanbul"); // Sipariş teslimatının yapılacağı şehir (Maksimum 20 karakterdir.Sadece alfabetik olabilir.)
		shippingAddress.setCitycode("34"); // Sipariş teslimatının yapılacağı şehir (Şehire ait ulusal veya uluslararası kod alanıdır. Türkiye için plaka kodu kullanılabilir.)
		shippingAddress.setZipcode("34580"); // Sipariş teslimatının yapılacağı posta kodu (Uluslararası posta kodu alanıdır.)
		shippingAddress.setDistrict("Şişli"); // Sipariş teslimatının yapılacağı ilçe (İlçe alanıdır.)
		shippingAddress.setDistrictCode(""); // Sipariş teslimatının yapılacağı ilçe kodu (İlçe kodu alanıdır.)
		shippingAddress.setShippingCompany("XXX"); // Taşıyıcı kargo bilgisi (Taşıyıcı kargo bilgisi alanıdır.)
		paymentRequest.setShippingaddress(shippingAddress);
		
		/**
		 * Invoice Address
		 * Fature Adresi
		 */
		InvoiceAddress invoceAddress = new InvoiceAddress();
		invoceAddress.setName("Ali Veli"); // Fatura kesilecek kişi veya kurum adı (Maksimum 40 karakterdir. Sadece alfanümerik karakterler, & . ve boşluk karakterini kabul eder.)
		invoceAddress.setAddress("Kuştepe Mahallesi Mecidiyeköy Yolu Cad. No:12 Trump Towers Kule:2 Kat:11 Şişli"); // Fatura adresi (Maksimum 500 karakterdir. Sadece alfanümerik karakterler, &.,/: ve boşluk karakterini kabul eder.)
		invoceAddress.setCountry("Türkiye"); // Fatura ülkesi (Maksimum 20 karakterdir. Sadece alfabetik olabilir.)
		invoceAddress.setCountrycode("TUR"); // Fatura ülkesi (2 veya 3 haneli ISO ülke kodudur. Sadece alfabetik olabilir.)
		invoceAddress.setCity("Istanbul"); // Fatura şehri (Maksimum 20 karakterdir. Sadece alfabetik olabilir.)
		invoceAddress.setCitycode("34"); // Fatura şehri (Şehire ait ulusal veya uluslararası kod alanıdır. Türkiye için plaka kodu kullanılabilir.)
		invoceAddress.setZipcode("34580"); // Fatura zip code (Uluslararası posta kodu alanıdır.)
		invoceAddress.setDistrict("Şişli"); // Sipariş teslimatının yapılacağı ilçe (İlçe alanıdır.)
		invoceAddress.setDistrictCode(""); // Sipariş teslimatının yapılacağı ilçe kodu (İlçe kodu alanıdır.)
		invoceAddress.setShippingCompany("XXX"); // Taşıyıcı kargo bilgisi (Taşıyıcı kargo bilgisi alanıdır.)
		paymentRequest.setInvoiceaddress(invoceAddress);
		
		/**
		 * Cart
		 * Sepet
		 */
		List basketList = new ArrayList<>();
		Basketitem firstBasketitem = new Basketitem();
		firstBasketitem.setDescription("Boyama Kalem Seti"); // Ürün ismi (Maksimum 40 karakterdir.)
		firstBasketitem.setProductcode("7cefdf61-38cd-4b35-b5f0-4c98c5805d41"); // Ürün kodu (Maksimum 36 karakterdir.)
		firstBasketitem.setAmount("87,50"); // Ürün fiyatı (Nokta ve virgülden arındırılmış double değerdir.)
		firstBasketitem.setVatratio("18"); // Tutarın KDV içerip içermediğini belirtir (0, 8 veya 18 değerlerini alabilir.)
		firstBasketitem.setCount("1"); // Ürün miktarı (Maksimum 3 haneli nümerik değerdir.)
		firstBasketitem.setUrl("http://www.alivelimarket.com.tr/boyama-kalem-seti"); // Ürün web adresi (Web URL'idir)
		firstBasketitem.setBasketItemType(BasketItemType.PHYSICAL.getCode()); // Ürün tipi (Ürünler için PHYSICAL, Kargo bilgisi için SHIPPING.)
		firstBasketitem.setBasketItemId("basket_1"); // Ürün kodu (Maksimum 40 karakterdir.)
		basketList.add(firstBasketitem);
		
		Basketitem secondBasketitem = new Basketitem();
		secondBasketitem.setDescription("Boyama Kitabı");
		secondBasketitem.setProductcode("7cefdf61-38cd-4b35-b5f0-4c98c5805d41");
		secondBasketitem.setAmount("25,50");
		secondBasketitem.setVatratio("18");
		secondBasketitem.setCount("3");
		secondBasketitem.setUrl("http://www.alivelimarket.com.tr/boyama-kitabi");
		secondBasketitem.setBasketItemType(BasketItemType.PHYSICAL.getCode());
		secondBasketitem.setBasketItemId("basket_2");
		basketList.add(secondBasketitem);
		
		Basketitem thirdBasketitem = new Basketitem();
		thirdBasketitem.setDescription("KargoBedeli");
		thirdBasketitem.setAmount("10,00");
		thirdBasketitem.setVatratio("18");
		thirdBasketitem.setCount("1");
		thirdBasketitem.setBasketItemType(BasketItemType.SHIPPING.getCode());
		thirdBasketitem.setBasketItemId("basket_3");
		basketList.add(thirdBasketitem);
		
		paymentRequest.setBasketitems(basketList);
		
		List extraList = new ArrayList<>();
		Extra extra1 = new Extra();
		extra1.setKey("INT_SPRS_KODU");
		extra1.setValue("spr_123456789");
		extraList.add(extra1);

		Extra extra2 = new Extra();
		extra2.setKey("INT_MUSTERI_KODU");
		extra2.setValue("mus_123456789");
		extraList.add(extra2);
		
		paymentRequest.setExtras(extraList);

		PaymentService paymentService = new PaymentService(hepsiPaySettings);
		PaymentResponse paymentResponse = paymentService.makePayment(paymentRequest);
		
		logger.debug(paymentResponse);
	}
	
	
}
```

#### Response
```JSON
{ 
  "Amount": 12300,
  "TransactionType": 0, 
  "Installment": 9, 
  "ApiKey": "1ca71cb09c7f4a2188fbddfa90efb481",
  "TransactionId": "tx_121345678912345", 
  "TransactionTime": "1447752023",
  "Signature": "8480954d54d94e5f272c53caa69efdcb0b678e837d3997eec42c4dbfa636cdde",
  "Currency": "TRY", 
  "Extras": [ { "Key": "INT_SPRS_KODU", "Value": "spr_123456789" } ],
  "Success": true, 
  "MessageCode": "0000", 
  "Message": "Başarılı", 
  "UserMessage": "İşleminiz başarıyla gerçekleştirildi."
  }
```

### 3D'li Satış İşlemi

#### Request

```Java

package com.hepsipay.sample;

import java.util.ArrayList;
import java.util.List;

import com.hepsipay.client.model.Basketitem;
import com.hepsipay.client.model.Card;
import com.hepsipay.client.model.Customer;
import com.hepsipay.client.model.Extra;
import com.hepsipay.client.model.InvoiceAddress;
import com.hepsipay.client.model.ShippingAddress;
import com.hepsipay.client.model.request.payment.Payment3D;
import com.hepsipay.client.service.PaymentService;
import com.hepsipay.client.type.BasketItemType;
import com.hepsipay.client.type.Currency;
import com.hepsipay.client.util.HepsiPayUtil;

/**
 * 
 * @author Ahmet Faruk Bişkinler,
 * @Support PixelTürk Web Studio, "destek@pixelturk.net"
 *
 */
public class SamplePayment3D extends Sample {
	
	public static void main(String[] args) {
		Payment3D payment3DRequest = new Payment3D();
		/**
		 * Create Payment3D Request
		 * Ödeme İsteği Oluştur
		 */
		//		payment3DRequest.setTransactiontime();    // Ödeme İşlem Zamanı, Varsayılan zaman ise şuan.
		//		payment3DRequest.setTransactionId("tx_" .  rand(100000, 999999));    // Ödemeye Ait Tekil Kodu (Maksimum 40 karakterdir)
		payment3DRequest.setTransactionId(HepsiPayUtil.generateRandomTranstionId()); // Ödemeye Ait Tekil Kodu (Maksimum 40 karakterdir)
		payment3DRequest.setDescription("E-Ticaret Ödemesi"); // Açıklama alanıdır (Maksimum 40 karakterdir)
		payment3DRequest.setAmount("120,99"); // Toplam Ödeme Tutarı
		payment3DRequest.setCurrency(Currency.TL.getCode()); // Toplam Ödeme Tutar Birimi
		payment3DRequest.setInstallment(1); // Ödeme Taksit Sayısı (1'den 12'ye kadar taksit değeridir)
		
		payment3DRequest.setSuccessUrl("http://example.com/payment_response.jsp?status=success"); // 3d secure işlemler için zorunlu bir alandır.İşlemin başarılı olması durumunda üye işyerine tarafından geliştirilmiş sayfanın URL'si yazılmalıdır.
		   payment3DRequest.setFailUrl("http://example.com/payment_response.jsp?status=fail"); // 3d secure işlemler için zorunlu bir alandır.İşlemin başarılı olması durumunda üye işyerine tarafından geliştirilmiş sayfanın URL'si yazılmalıdır.
		
		/**
		 * Credit Card Info
		 * Kredi Kartı Bilgileri
		 */
		Card card = new Card();
		card.setCardHolderName("John Doe"); // Kart İsim Soyisim (Maksimum 40 karakterdir. Sadece alfabetik karakterler ve boşluk kabul eder)
		card.setCardNumber("4355084355084358"); // Kart Numarası (15 veya 16 haneli nümerik değerdir)
		card.setExpireMonth("12"); // Kart Son Kullanım Tarihi (2 haneli nümerik değerdir.)
		card.setExpireYear("18"); // Kart Son Kullanım Tarihi (2 haneli nümerik değerdir.)
		card.setSecuritycode("000"); // Kart CVV (3 veya 4 haneli nümerik değerdir.)
		payment3DRequest.setCard(card);
		
		/**
		 * Customer Info
		 * Müşteri Bilgileri
		 */
		Customer customer = new Customer();
		customer.setName("Ali"); // Müşteri İsim (Maksimum 20 karakterdir. Sadece alfabetik karakterler ve boşluk kabul eder.)
		customer.setSurname("Veli"); // Müşteri Soyisim (Maksimum 20 karakterdir. Sadece alfabetik karakterler ve boşluk kabul eder.)
		customer.setEmail("ali.veli@alivelimarket.com.tr"); // Müşteri Mail (Eposta adresidir.)
		customer.setIpaddress("72.125.165.16"); // Müşteri IP Adresi (IP adresidir.)
		customer.setPhonenumber("+905350000000"); // Müşteri Telefon Numarası (Maksimum 13 karakterdir. Sadece nümerik ve + değerlerini alabilir.)
		customer.setCode("7cefdf61-38cd-4b35-b5f0-4c98c5805d41"); // Müşteri Kodu (Maksimum 36 karakterdir.)
		customer.setTckn("12345678910"); // Müşteri Kimik Numarısı (TC Kimlik numarasıdır.)
		customer.setVatNumber("12345678910"); // Müşteri Vergi Numarası (Şirketlere ve firmalara tekil vergi numarası)
		customer.setMemberSince("20151124"); // Müşteri Üyelik tarihi (Müşteri üyelik tarihi formatı YYYYMMDD’dır.)
		customer.setBirthdate("19781123"); // Müşteri Doğum tarihi (Müşteri üyelik tarihi formatı YYYYMMDD’dır.)
		payment3DRequest.setCustomer(customer);
		
		/**
		 * Shipping Address
		 * Teslimat/Nakliye Adresi
		 */
		ShippingAddress shippingAddress = new ShippingAddress();
		shippingAddress.setName("Ali Veli"); // Sipariş Teslimatının yapılacağı kişi adı ve soyadı (Maksimum 40 karakterdir. Sadece alfabetik karakterler ve boşluk kabul eder.)
		shippingAddress.setAddress("Kuştepe Mahallesi Mecidiyeköy Yolu Cad. No:12 Trump Towers Kule:2 Kat:11 Mecidiyeköy"); // Siparişin teslim edileceği adres (Maksimum 500 karakterdir. Sadece alfanümerik karakterler, &.,/: ve boşluk karakterini kabul eder.)
		shippingAddress.setCountry("Türkiye"); // Sipariş teslimatının yapılacağı ülke (Maksimum 20 karakterdir. Sadece alfabetik olabilir.)
		shippingAddress.setCountrycode("TUR"); // Sipariş teslimatının yapılacağı ülke (2 veya 3 haneli ISO ülke kodudur.Sadece alfabetik olabilir.)
		shippingAddress.setCity("Istanbul"); // Sipariş teslimatının yapılacağı şehir (Maksimum 20 karakterdir.Sadece alfabetik olabilir.)
		shippingAddress.setCitycode("34"); // Sipariş teslimatının yapılacağı şehir (Şehire ait ulusal veya uluslararası kod alanıdır. Türkiye için plaka kodu kullanılabilir.)
		shippingAddress.setZipcode("34580"); // Sipariş teslimatının yapılacağı posta kodu (Uluslararası posta kodu alanıdır.)
		shippingAddress.setDistrict("Şişli"); // Sipariş teslimatının yapılacağı ilçe (İlçe alanıdır.)
		shippingAddress.setDistrictCode(""); // Sipariş teslimatının yapılacağı ilçe kodu (İlçe kodu alanıdır.)
		shippingAddress.setShippingCompany("XXX"); // Taşıyıcı kargo bilgisi (Taşıyıcı kargo bilgisi alanıdır.)
		payment3DRequest.setShippingaddress(shippingAddress);
		
		/**
		 * Invoice Address
		 * Fature Adresi
		 */
		InvoiceAddress invoceAddress = new InvoiceAddress();
		invoceAddress.setName("Ali Veli"); // Fatura kesilecek kişi veya kurum adı (Maksimum 40 karakterdir. Sadece alfanümerik karakterler, & . ve boşluk karakterini kabul eder.)
		invoceAddress.setAddress("Kuştepe Mahallesi Mecidiyeköy Yolu Cad. No:12 Trump Towers Kule:2 Kat:11 Şişli"); // Fatura adresi (Maksimum 500 karakterdir. Sadece alfanümerik karakterler, &.,/: ve boşluk karakterini kabul eder.)
		invoceAddress.setCountry("Türkiye"); // Fatura ülkesi (Maksimum 20 karakterdir. Sadece alfabetik olabilir.)
		invoceAddress.setCountrycode("TUR"); // Fatura ülkesi (2 veya 3 haneli ISO ülke kodudur. Sadece alfabetik olabilir.)
		invoceAddress.setCity("Istanbul"); // Fatura şehri (Maksimum 20 karakterdir. Sadece alfabetik olabilir.)
		invoceAddress.setCitycode("34"); // Fatura şehri (Şehire ait ulusal veya uluslararası kod alanıdır. Türkiye için plaka kodu kullanılabilir.)
		invoceAddress.setZipcode("34580"); // Fatura zip code (Uluslararası posta kodu alanıdır.)
		invoceAddress.setDistrict("Şişli"); // Sipariş teslimatının yapılacağı ilçe (İlçe alanıdır.)
		invoceAddress.setDistrictCode(""); // Sipariş teslimatının yapılacağı ilçe kodu (İlçe kodu alanıdır.)
		invoceAddress.setShippingCompany("XXX"); // Taşıyıcı kargo bilgisi (Taşıyıcı kargo bilgisi alanıdır.)
		payment3DRequest.setInvoiceaddress(invoceAddress);
		
		/**
		 * Cart
		 * Sepet
		 */
		List basketList = new ArrayList<>();
		Basketitem firstBasketitem = new Basketitem();
		firstBasketitem.setDescription("Boyama Kalem Seti"); // Ürün ismi (Maksimum 40 karakterdir.)
		firstBasketitem.setProductcode("7cefdf61-38cd-4b35-b5f0-4c98c5805d41"); // Ürün kodu (Maksimum 36 karakterdir.)
		firstBasketitem.setAmount("87,50"); // Ürün fiyatı (Nokta ve virgülden arındırılmış double değerdir.)
		firstBasketitem.setVatratio("18"); // Tutarın KDV içerip içermediğini belirtir (0, 8 veya 18 değerlerini alabilir.)
		firstBasketitem.setCount("1"); // Ürün miktarı (Maksimum 3 haneli nümerik değerdir.)
		firstBasketitem.setUrl("http://www.alivelimarket.com.tr/boyama-kalem-seti"); // Ürün web adresi (Web URL'idir)
		firstBasketitem.setBasketItemType(BasketItemType.PHYSICAL.getCode()); // Ürün tipi (Ürünler için PHYSICAL, Kargo bilgisi için SHIPPING.)
		firstBasketitem.setBasketItemId("basket_1"); // Ürün kodu (Maksimum 40 karakterdir.)
		basketList.add(firstBasketitem);
		
		Basketitem secondBasketitem = new Basketitem();
		secondBasketitem.setDescription("Boyama Kitabı");
		secondBasketitem.setProductcode("7cefdf61-38cd-4b35-b5f0-4c98c5805d41");
		secondBasketitem.setAmount("25,50");
		secondBasketitem.setVatratio("18");
		secondBasketitem.setCount("3");
		secondBasketitem.setUrl("http://www.alivelimarket.com.tr/boyama-kitabi");
		secondBasketitem.setBasketItemType(BasketItemType.PHYSICAL.getCode());
		secondBasketitem.setBasketItemId("basket_2");
		basketList.add(secondBasketitem);
		
		Basketitem thirdBasketitem = new Basketitem();
		thirdBasketitem.setDescription("KargoBedeli");
		thirdBasketitem.setAmount("10,00");
		thirdBasketitem.setVatratio("18");
		thirdBasketitem.setCount("1");
		thirdBasketitem.setBasketItemType(BasketItemType.SHIPPING.getCode());
		thirdBasketitem.setBasketItemId("basket_3");
		basketList.add(thirdBasketitem);

		payment3DRequest.setBasketitems(basketList);

		List extraList = new ArrayList<>();
		Extra extra1 = new Extra();
		extra1.setKey("INT_SPRS_KODU");
		extra1.setValue("spr_123456789");
		extraList.add(extra1);

		Extra extra2 = new Extra();
		extra2.setKey("INT_MUSTERI_KODU");
		extra2.setValue("mus_123456789");
		extraList.add(extra2);
		
		payment3DRequest.setExtras(extraList);
		
		PaymentService payment3DService = new PaymentService(hepsiPaySettings);
		payment3DService.makePayment3D(payment3DRequest);
	}
	
}

```

#### Response
```JSON
{ 
  "Amount": 12300,
  "TransactionType": 0, 
  "Installment": 1, 
  "ApiKey": "1ca71cb09c7f4a2188fbddfa90efb481",
  "TransactionId": "tx_121345678912345", 
  "TransactionTime": "1447752023",
  "Signature": "8480954d54d94e5f272c53caa69efdcb0b678e837d3997eec42c4dbfa636cdde",
  "Currency": "TRY", 
  "Extras": [ { "Key": "INT_SPRS_KODU", "Value": "spr_123456789" } ],
  "Success": true, 
  "MessageCode": "0000", 
  "Message": "Başarılı", 
  "UserMessage": "İşleminiz başarıyla gerçekleştirildi."
  }
```

### İade İşlemi

#### Request

```Java

apackage com.hepsipay.sample;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

import com.hepsipay.client.model.request.payment.Refund;
import com.hepsipay.client.model.response.RefundResponse;
import com.hepsipay.client.service.RefundService;
import com.hepsipay.client.type.Currency;
import com.hepsipay.client.util.HepsiPayUtil;

/**
 * 
 * @author Ahmet Faruk Bişkinler,
 * @Support PixelTürk Web Studio, "destek@pixelturk.net"
 *
 */
public class SampleRefund extends Sample {
	private final static Logger logger = LogManager.getRootLogger();
	
	public static void main(String[] args) {
		Refund refundRequest = new Refund();
		refundRequest.setTransactionId(HepsiPayUtil.generateRandomTranstionId()); // Ödemeye Ait Tekil Kodu (Maksimum 40 karakterdir)
		refundRequest.setReferenceTransactionId("tx_466943"); // İade Edilecek Ödemeye Ait Tekil Kod (Maksimum 40 karakterdir)
		refundRequest.setAmount("1,00"); // Toplam İade Tutarı
		refundRequest.setCurrency(Currency.TL.getCode()); // Toplam Ödeme Tutar Birimi
		
		RefundService refundService = new RefundService(hepsiPaySettings);
		RefundResponse refundResponse = refundService.makeRefund(refundRequest);
		
		logger.debug(refundResponse);
	}
	
}


```

#### Response
```JSON

{
  "ApiKey": "1ca71cb09c7f4a2188fbddfa90efb481",  
  "TransactionId": " tx_123456789",  
  "ReferenceTransactionId": "tx_12345678",  
  "TransactionTime": "1447752023",  
  "Signature": "8480954d54d94e5f272c53caa69efdcb0b678e837d3997eec42c4dbfa636cdde",  
  "Currency": "TRY", 
  "amount": 100,  
  "MessageCode": "0000",  
  "Message": "Başarılı",
  "UserMessage": "İşleminiz başarıyla gerçekleştirildi"  
}  


```

### Test Kartları


Kart No          | Banka        | Son Kullanma Tarihi	| CVV		| 3D Secure
-----------      | ----         | -------------------	| ---    	| ---------				
4531444531442283 | Halkbank     | 12/18 				| 001  	 	| Var
5818775818772285 | Halkbank     | 12/18  				| 001	 	| Var
9792004525458548 | Halkbank     | 12/20                 | 001 	 	| Var
5571135571135575 | Akbank       | 12/18                 | 000		| Var
4355084355084358 | Akbank       | 12/18                 | 000       | Var
9792087721232551 | Akbank       | 12/20                 | 000       | Var
375622005485014  | Garanti		| 07/19				    | 123       | Var
4282209004348015 | Garanti		| 07/19				    | 123       | Var
4282209027132016 | Garanti		| 05/18			        | 358       | Yok
4824894728063019 | Garanti		| 06/17				    | 959       | Yok
5289394722895016 | Garanti		| 08/17			        | 820       | Var
5549604734932029 | Garanti		| 03/18		            | 119       | Yok
4508034508034509 | İşbank       | 12/20				    | 000       | Var
5406675406675403 | İşbank       | 12/20				    | 000       | Var
9792042022562362 | İşbank       | 12/20				    | 000       | Var
5400610093155852 | Yapı Kredi	| 02/20				    | 000       | Var
5400617049774124 | Yapı Kredi	| 02/20				    | 000       | Var
5400637003737156 | Yapı Kredi	| 02/20				    | 000       | Var
4506347011448053 | Yapı Kredi	| 02/20				    | 000       | Var
4506347022052795 | Yapı Kredi	| 02/20				    | 000       | Var
4506347031187533 | Yapı Kredi	| 02/20				    | 000       | Var
4506347043358536 | Yapı Kredi	| 02/20				    | 000       | Var
4022774022774026 | FinansBank	| 12/18				    | 000       | Var
5456165456165454 | FinansBank	| 12/18				    | 000       | Var
9792031125125565 | FinansBank	| 12/20				    | 000       | Var

### Cevap Kodları

Cevap Kodu		     | Kullanıcı Mesajı																															| Teknik Mesaj
-------------------  | -----------------								  																						| -------------
0000                 | İşleminiz başarıyla gerçekleşmiştir.				  																						| İşleminiz başarıyla gerçekleşmiştir.
1007		         | Lütfen girdiğiniz kart bilgilerini kontrol edip tekrar deneyiniz.																		| Geçersiz güvenlik kodu.
1009                 | Lütfen girdiğiniz kart bilgilerini kontrol edip tekrar deneyiniz.																		| Kart bilgisi geçersiz.
1021                 | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.																| Sanal Pos Banka tarafında kapalıdır
1028		         | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.																| İadesi Yapılmış İşlem Tekrar İade Edilemez.
1029                 | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.																| Kart Hatası.
1501                 | Kartınızın yetkileri ya da kısıtlamalarından dolayı hata almaktasınız.Kartınızın bankası ile iletişime geçmenizi rica ederiz.			| Kart Hatası.
1503		         | Lütfen girdiğiniz kart bilgilerini kontrol edip tekrar deneyiniz.																		| Kart bilgisi geçersiz.
1504                 | Kartınız online işleme kapalıdır. Kartınız online işlemlere açıldığında tekrar denemenizi rica ederiz.									| Kart Hatası
1505                 | Kartınızın yetkileri ya da kısıtlamalarından dolayı hata almaktasınız.Kartınızın bankası ile iletişime geçmenizi rica ederiz.            | Kart Hatası
1506		         | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.							                                    | Tanım Hatası
1507                 | Kartınızın yetkileri ya da kısıtlamalarından dolayı hata almaktasınız.Kartınızın bankası ile iletişime geçmenizi rica ederiz.   			| Kart Hatası
1508                 | Kartınızın yetkileri ya da kısıtlamalarından dolayı hata almaktasınız.Kartınızın bankası ile iletişime geçmenizi rica ederiz. 	        | Kart Hatası
1517		         | Kartınızın yetkileri ya da kısıtlamalarından dolayı hata almaktasınız.Kartınızın bankası ile iletişime geçmenizi rica ederiz.            | Kart Hatası
1518                 | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Tanım Hatası
1520                 | Kartınız üzerinde bu işlem için yeterli bakiyeniz bulunmamaktadır.                                                                       | Hesap müsait değil.
1521		         | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Tanım Hatası
1523                 | İzin verilen şifre giriş sayısı aşıldı.Kartınızın bankası ile iletişime geçmeniz gerekmektedir.                        	                | Kart Hatası
1525                 | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Geçersiz PIN.
1526		         | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | ARQC hatası.
1530                 | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Kart Hatası
1532                 | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Banka İşlemlere Cevap Vermiyor
1544                 | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Tanım Hatası
1577		         | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Kredi kartı blacklist'te bulunmaktadır.
3025                 | Kartınız taksitli işlemlere kapalı olduğu için işleminiz gerçekleştirilemiyor.Lütfen bankanız ile iletişime geçiniz.                     | Kredi kartı taksitli işleme kapalı.
3032                 | Lütfen girdiğiniz kart bilgilerini kontrol edip tekrar deneyiniz.                                                                        | Kart bilgileri hatalı.
3065		         | Eklenmek istenilen kredi kartı daha önceden eklendiği için işleminizi gerçekleştiremiyoruz. Lütfen yeni bir kart numarası ekleyiniz.     | Kaydedilmek istenilen kart daha önceden kaydedilmiş.
3066                 | Aradığınız kriterlere uygun sonuç bulunamadı.		                                                                                    | Aranan değerlere göre kurumunuza ait kayıtlı kart bulunamamıştır.
1000,1001,1003,1015  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
1016,1020,1025,1032  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası 
1032,1041,1043,1510  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
1511,1513,1514,1522  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
1527,1528,1531,1533  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
1534,1535,1536,1537  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
1539,1540,1541,1542  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
1546,1547,1548,1549  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
1550,1551,1553,1554  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
1559,1560,1561,1562  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
1563,1567,1568,1569  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
1571,1574,1575,1576  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
1578,1579,1580,1581  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
1582,1583,2000,2011  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
3093,3094,4022,4023  | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası
4024,4027,4032, 4034 | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası 
6000				 | Şu anda işleminizi gerçekleştiremiyoruz. Lütfen daha sonra tekrar deneyiniz.                                                             | Genel sistem hatası 

```
