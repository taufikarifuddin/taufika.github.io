# IMPLEMENTASI UNIT TEST PADA SPRING BOOT

Sebelumnya, telah menganai apa itu [Unit Test](https://github.com/taufikarifuddin/taufikarifuddin.github.io/blob/master/materi/id/unittest.md) dan beberapa Pro-Const mengenai unit test. Kali ini yang akan dibahas adalah bagaimana mengimplementasikan hal tersebut pada project yang menggunakan [Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection) seperti halnya Spring Boot ?. Mungkin ada beberapa yang bisa digunakan, salah satunya adalah menggunakan Mocking Framework untuk melakukan Mocking pada dependency tersebut. 

# Apa yang akan di bahas ? 
1. [Mocking Framework](#Mocking-Framework)
2. [Implementasi Unit Test pada Spring Boot](#Implementasi-Unit-Test-pada-Spring-Boot)

## Mocking Framework
Mocking Framework digunakan melakukan mock / replacement terhadap sebuah object. Mock sendiri menurut [Wikipedia](https://en.wikipedia.org/wiki/Mock_object) adalah sebuah simulasi object yang sama seperti object aslinya tetapi dapat dicontrol atau diekspektasikan. Singkatnya, kita dapat melakukan ekspektasi / control terhadap pemanggilan method dan return valuenya.

## Implementasi Unit Test pada Spring Boot

Disini, untuk mocking framework saya menggunakan Mockito sebagai Mocking Framework. 

Apa saja hal yang dapat dilakukan oleh Mockito ? 
1. Dapat melakukan mocking terhadap object yang sudah di mock sebelumnya
2. Dapat melakukan `verify` terhadap method-method / operasi yang digunakan dari objectyang telah diMock sebelumnya.
3. Dapat memastikan bahwa `verify` yang dilakukan sudah valid, Seperti  method dari object Mock yang digunakan dalam proses, berapa kali method dari object mock dipanggil

dan masih banyak lagi, tapi kegunaan diatas yang akan dibahas pada artikel ini.

Untuk case kali ini terdapat sebuah service yang digunakan untuk melakukan mengambil data dari user user dan memiliki dependency terhadap repository file, sebagai berikut :  
```
@Service
public class UserServiceImpl implements UserService{
		
	@Autowired
	private UserRepository userRepository;
	
	@Override
	public User getUserData(String identifier) throws BadRequestExeption {
        User user = Optional
				.ofNullable(userRepository.findByUsername(identifier))
				.orElseGet(() -> userRepository.findByEmail(identifier));

		if(user == null){
			throw new BadRequestExeption("User Not Found !!");
		}

		return user;
	}

}
```
Lalu apa yang akan kita lakukan untuk melakukan Unit Test pada service tersebut :
1. Melakukan Mock pada Dependency dari service tersebut

    Pada potongan code diatas, menunjukkan bahwa class tersebut memiliki dependency terhadap UserRepository. Untuk melakukan mock terhadap dependency tersebut, diperlukan bantuan dari Mockito untuk melakukan mock terhadap object dependency tersebut. Untuk melakukannya kita hanya perlu mengetahui dependency yang akan dimock dan dependency tersebut akan dimock kedalam class apa, seperti berikut :  
    ```
        @InjectMocks //class yang dependencynya akan di mock
        private UserServiceBean userService;

        @Mock //dependency yang ingin di mock
        private UserRepository userRepo;
    ```

    mengapa kita perlu melakukan mock terhadap dependencynya ?. Setelah melihat snipped code service diatas, object UserRepository diset melalui Dependency Injection oleh Spring dan memiliki access modifier private. Jadi ketika kita tidak melakukan mock pada object tersebut maka akan terjadi `NullPointerException` ketika melakukan operasi `userRepository.findByUsername(identifier)`.

2. Mocking Dependency yang telah di Mock
    Mock yang dimaksudkan ini dalah sebuah object yang dapat diekspektasikan terhadap pemanggilannya dan return valuenya. seperti pada contoh di bawah ini : 
    ```
    when(userRepo.findByUsername("userEmail@mail.com")).thenReturn(null);
    ```
    snippet code diatas kita melakukan mocking pada object UserRepository yang sudah kita mock sebelumnya. Pada contoh diatas kita berekspektasi bahwa ketika method `findByUsername` dari class `UserRepository` dengan parameter identifier `userEmail@mail.com` maka akan memberikan return value `null`. 

    Dengan melakukan mock kita dapat melakukan banyak test case. seperti : 
    - user melakukan find data yang ternyata menggunakan username
    - user melakukan find data yang ternyata menggunakan email
3. Tulis Unit Test
    
   Untuk Unit test terhadap test case diatas adalah sebagai berikut 
   ```
   @Test
	public void getUserData_usingUsername() throws BadRequestExeption {
	 	User user = new User();
	 	user.setFullName("fullName");
	 	user.setEmail("userEmail@mail.com");

	 	when(userRepo.findByUsername("username")).thenReturn(user);

		User response =
				userService.getUserData("username");

		assertEquals("fullName", response.getFullName());
		assertEquals("userEmail@mail.com", response.getEmail());

		verify(userRepo).findByUsername("username");
	}


	@Test
	public void getUserData_usingEmail() throws BadRequestExeption {
		User user = new User();
		user.setFullName("fullName");
		user.setEmail("userEmail@mail.com");

		when(userRepo.findByEmail("userEmail@mail.com")).thenReturn(user);

		User response =
				userService.getUserData("userEmail@mail.com");

		assertEquals("fullName", response.getFullName());
		assertEquals("userEmail@mail.com", response.getEmail());

		verify(userRepo).findByUsername("userEmail@mail.com");
		verify(userRepo).findByEmail("userEmail@mail.com");
	}

    @Test(expected = BadRequestExeption.class)
	public void getUserData_notFound() throws BadRequestExeption {
	 	try {
			userService.getUserData("username");
		}catch (BadRequestExeption e) {
	 		assertEquals("User Not Found !!", e.getMessage());
			verify(userRepo).findByEmail("username");
			verify(userRepo).findByUsername("username");
			throw  e;
		}
	}
   ```
   Dari keterangan unit test diatas, untuk test case :
   1. `getUserData_usingUsername`, identifier yang diberikan oleh user sebagai parameter adalah username. Mengapa pada snipper diatas tidak memerlukan mocking terhdap email ? karena ekspektasi dari snippet service adalah tidak melakukan operasi `userRepo.findByEmail` karena user akan menemukan datanya ketika melakukan operasi `userRepo.findByUsername`. Maka dari itu hanya perlu memerlukan mock pada  `userRepo.findByUsername` dan melakukan `verify` terhadap `userRepo.findByUsername` karena ekspektasinya tidak melakukan operasi `userRepo.findByEmail`.

   2. `getUserData_usingEmail`, identifier yang diberikan oleh user sebagai parameter adalah email. Dari logika yang ada di snippet service, `userRepo.findByEmail` akan dipanggil ketika `userRepo.findByUsername` tidak menemukan datanya. Sehingga ekspektasinya bahwa ketika melakukan operasi `userRepo.findByUsername` user tidak ditemukan. Sehingga unit testnya dapat melakukan mocking dengan `when(userRepo.findByUsername("userEmail@mail.com")).thenReturn(null)` atau  tidak melakukan mocking terhadap operasi tersebut sama sekali dan hanya melakukan mocking pada `userRepo.findByEmail` saja.

        Terdapat beberapa perbedaan dari test case diatas yaitu melakukan 2 verify untuk `findByUsername` dan `findByEmail`. Mengapa hal tersebut diperlukan ? karena berdasarkan logika yang sudah ditulis, ketika user tidak menemukan datanya ketika `findByUsername` maka akan melakukan operasi `findByEmail` sehinggal kedua operasi tersebut dipanggil. Maka dari itu kita perlu untuk melakukan verify terhadap kedua operasi tersebut bahwa kedua operasi tersebut benar-benar dipanggil.

   3. `getUserData_notFound`. Berbeda dari test case sebelumnya, user tidak menemukan datanya dari ketika melakukan pencarian dengan username ataupun email. Untuk test case kali ini kita tidak memerlukan Mocking karena memang kita berekspektasi untuk menemukan user pada operasi `findByUsername` dan `findByEmail`. 

        Result dari Test case ini adalah Exception `BadRequestExeption` karena user yang dicari tidak ditemukan. So kenapa kita memerlukan `try catch` ? kenapa tidak hanya  menulis seperti berikut :  
        ```
        @Test(expected = BadRequestExeption.class)
        public void getUserData_notFound() throws BadRequestExeption {
                userService.getUserData("username");
        
        }
        ```
        seperti halnya dalam [Pengenalan Unit Test](https://github.com/taufikarifuddin/taufikarifuddin.github.io/blob/master/materi/id/unittest.md).

        mungkin ada beberapa cara untuk melakukan verify untuk case yang terjadi diatas. Tapi seperti ini yang biasa saya lakukan. Mengapa ? 
        karena pada `@After` method saya menggunakan `Mockito.verifyNoMoreInteractions` untuk melakukan verify terhadap semua verification yang teah dilakukan pada unit test. Seperti berikut : 
        ```
        @After
        public void tearDown() {
            Mockito.verifyNoMoreInteractions(this.userRepo);
        }

        ```
        ketika tidak dilakukan hal tersebut maka Unit Test akan gagal dikarenakan ada pemanggilan method dari object Mock yaitu `userRepo.findByEmail` dan `userRepo.findByUsername` yang tidak dilakukan `verify`, detail errornya adalah sebagai berikut : 
        ```
        org.mockito.exceptions.verification.NoInteractionsWanted: 
        No interactions wanted here:
        -> at com.gdn.paperless.service.UserServiceTest.finishTest
        But found this interaction on mock 'userRepo':
        -> at com.gdn.paperless.service.user.UserServiceImpl.getUserData
        ```
        maka dari itu perlu adanya `try catch` untuk melakukan `verify` terhadap operasi object mock yang dipanggil. 
