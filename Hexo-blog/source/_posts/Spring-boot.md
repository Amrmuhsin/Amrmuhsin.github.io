---
title: Java Spring-boot
tags:
	- Blog Intern
categories:
	- Artivisi
commands: true
date: 2017-08-15 11:35:45
---

## Learning Java Spring-boot

<h3>Create Project</h3>
Kita bisa langsung membuat project srping-boot melalui

    start.spring.io

disana kita tinggal mengisi nama project dan yang lainnya dan jangan lupa untuk mengisi depedency JPA dan Mysql,
setelah itu projectnya tinggal di download dan di buka melalui netbeans atau applikasi lainnya.

<h3>Maven</h3>

Menggunakan maven, kita bisa menggunakan maven dengan cara menambahkan depedency Apache Maven ke dalam Pom.xml

	<dependency>
 		<groupId>org.apache.maven.plugins</groupId>
  		<artifactId>maven-dependency-plugin</artifactId>
  		<version>3.0.1</version>
  		<type>maven-plugin</type>
  	</dependency>

Buat package di dalam project java supaya lebih rapi, di sana ada Entity, Dao dan Controller package
- Entity class dengan annotation @Entity

Contoh: 

 	@Entity
 	public class Peserta {
 	@Id
 	private String Id;

 	@Column(nullable = false)
 	private String nama;

 	@Column(nullable = false, unique = true)
 	private String email;
 	}

Full source code class Peserta here : <a href="http://localhost:4000/2017/07/07/Java%20Code/Peserta/" title="Peserta">Peserta Java Code</a>

LALU MENYIAPKAN USER DAN PASS UNTUK KONEKSI DATABASE
edit application.properties
* spring.datasource.url = jdbc:mysql://localhost:3306/namadatabase
* spring.datasource.username = root
* spring.datasource.password = passwordkamu

Full source code Application.properties here : <a href="http://localhost:4000/2017/07/07/Java%20Code/ApplicationProperties/" title="Application.Properties ">Application.properties Java Code</a>

<h3>Buat Database</h3>
Setelah itu create database sesuai dengan nama database yang di edit di application.properties tadi, dan beri akses ke username dan password yang sama di app.pro

    * mysql -u root-p password : untuk masuk ke dalam mysql
    * grant all on namadatabase.* to root@localhost identified by 'passwordkamu': untuk memberi akses ke database
    * create database namadatabase : membuat database
    * use namadatabase : menggunakan database
    * show tables : untuk menampilkan table yang ada didatabase yang kita gunakan
     misal table yang ada adalah table peserta
    * show create table peserta /G : sebelum menjalankan perintah ini harus mengisi data yang ada di entity peserta terlebih dahulu, dan begitu seterusnya jika kita memuat entity baru (*disarankan untuk menghapus terlebih dahulu table yang kita buat agar tidak terjadi konflik apabila id yang kita gunakan unique).

JALANKAN PROJECT

* mvn clean project
* mvn clean spring-boot:run

<h5>PesertaController</h5>

Contoh cariPeserta :

    @Autowired
    private PesertaDao pd;
    @RequestMapping(value="/peserta", method = RequestMethod.GET)
    public Page<Peserta> cariPeserta(Pageable page){
        return pd.findAll(page);
    }

Contoh insertPesertaBaru :

    @RequestMapping(value="/peserta", method = RequestMethod.POST)
    @ResponseStatus(HttpStatus.CREATED)
    public void insertPesertaBaru(@RequestBody @Valid Peserta p){
        pd.save(p);
    }
    
Contoh updatePeserta :

    @RequestMapping(value="/peserta/{id}", method = RequestMethod.PUT)
    @ResponseStatus(HttpStatus.OK)
      public void updatePeserta(@PathVariable("id") String id, @RequestBody @Valid Peserta p){
        p.setId(id);        //penamabahan @Valid Untuk memvalidasi input sesuai dgn yg di inginkan
        pd.save(p);
      }
      
Contoh findPeserta :

    @RequestMapping(value="/peserta/{id}", method = RequestMethod.GET)
    @ResponseStatus(HttpStatus.OK)
      public ResponseEntity<Peserta> cariPesertaById(@PathVariable ("id") String id){
          Peserta hasil = pd.findOne(id);
          if(hasil == null){
              return new ResponseEntity<>(HttpStatus.NOT_FOUND);
          }
          return new ResponseEntity<>(hasil, HttpStatus.OK);
      }
      
Contoh hapusPeserta :

    @RequestMapping(value="/peserta/{id}", method = RequestMethod.DELETE)
    @ResponseStatus(HttpStatus.OK)
    public void hapusPeserta(@PathVariable("id") String id) {
        pd.delete(id);
    }

Full source code class PesertaController here : <a href="http://localhost:4000/2017/07/07/Java%20Code/PesertaController/" title="PesertaController">PesertaController Java Code</a>

Contoh PesertaDao :

    public interface PesertaDao extends PagingAndSortingRepository<Peserta, String > {

    }

Full source code class PesertaDao here : <a href="http://localhost:4000/2017/07/07/Java%20Code/PesertaDao/" title="PesertaDao">PesertaDao Java Code</a>

Contoh Entity Peserta :

    @Entity
    public class Peserta {
        @Id @GeneratedValue(generator = "uuid")
        @GenericGenerator(name = "uuid", strategy = "uuid2")
        private String id;
        @Column(nullable = false)
        @NotNull       //VALIDASI
        @NotEmpty      //VALIDASI
        @Size(min = 3, max = 150)
        private String nama;
        @Column(nullable = false, unique = true)
        @Email      //Untuk ngasih tau bahwa ini adalah email
        @NotNull    //VALIDASI
        @NotEmpty   //VALIDASI
        private String email;
        @Column(name = "tanggal_lahir", nullable = false)
        @Temporal(TemporalType.DATE)
        @Past
        @NotNull    //VALIDASI
        private Date tanggalLahir;

jangan lupa memasukkan setter and getter di entity
Full source code class Peserta here : <a href="http://localhost:4000/2017/07/07/Java%20Code/Peserta/" title="Peserta">Peserta Java Code</a>

<h3>Spring-boot Web and RESTful</h3>

Untuk menambahkan aplikasi web di spring-boot yang harus dilakukakan adalah mengimport atau memasukkan depedency ke dalam pom.xml

	<depedency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	</depedency>

dan buat folder static di src/main/java/resouces lalu buat file.hmtl di src/main/java/resources/static

kita juga perlu membuat satu folder lagi di dalam resources, buat folder js
jadi di src/main/java/resources/js kita buat satu file pelatihan.js

	var aplikasiPelatihan = angular.module('aplikasiPelatihan', []);

	aplikasiPelatihan.controller('HaloController', function($scope){
    $scope.daftarEmail = [
        'amr.muhsin@gmail.com',
        'amr.muhsin@artivisi.com',
        'amr.muhsin@yahoo.com'
    ];
    
    $scope.tambahEmail = function(){
        $scope.daftarEmail.push($scope.email);
        //$scope.email = "";
    };
    
    $scope.hapusEmail = function(x){
        var lokasiIndex = $scope.daftarEmail.indexOf(x);
        if (lokasiIndex > -1){
            $scope.daftarEmail.splice(lokasiIndex, 1);
            }
        };
	}	);

	aplikasiPelatihan.controller('MateriController', function($http, $scope){
    $scope.dataMateri ={};
    
    $scope.simpanMateri = function(){
        $http.post('/api/materi', $scope.materi).then(sukses, gagal);
        
        function sukses(response){
           $scope.updateDataMateri();
        }
        
        function gagal(response){
            console.log(response);
            alert('Error : '+response);
        };
    
    };
    
    $scope.hapusMateri = function(x){
        $http.delete('/api/materi/' +x.id).then(sukses, gagal);
         
        function sukses(response){
           $scope.updateDataMateri();
        }
        
        function gagal(response){
            console.log(response);
            alert('Error : '+response);    
        };
    };
    
    $scope.updateDataMateri = function(){
        $http.get('/api/materi').then(sukses, gagal);
        
        function sukses(response){
            $scope.dataMateri = response.data;
            console.log($scope.dataMateri);
        };
        
        function gagal(response){
            console.log(response);
            alert('Error : '+response);    
        };
        
    };
    
    $scope.updateDataMateri();
	}); 
Full source code class Peserta here : <a href="http://localhost:4000/2017/07/07/Java%20Code/Pelatihanjs/" title="Pelatihan.js">Pelatihan.js Java Code</a>

<h3>Spring-boot RESTful API</h3>

	@RestController
	public class PesertaController

	@Autowired
	private PesertaDao pd;

	@RequestMapping(value="/peserta", methog = RequestMethod.GET)
	public Page<Peserta> cariPeserta(Pageable Page) {
		return.pd.findAll(page);
	}


Menggunakan RESTful di chrome kita bisa menggunakan POSTMAN, disana kita bisa lihat respond method apapun seperti GET, PUT, POST ataupun DELETE. kalo kita mau menggunakan POST method kita bisa menggunakan json dan memasukkan body data seperti nama dan email.

    @RequestMapping : dipasang di method, untuk melakukan mapping diantara URL dengan method yang akan menanganinya.
    @RequestParam : di pasang di method argument, Untuk mengambil data/variabel dari query parameter. Misalnya 'peserta?nama=endy'
    @PathVariable : dipasang di method argument, untuk mengambil data/variabel dari URL. Misalnya {"sesi/JF-001/2015"}
    @ResponseStatus :  menyatakan HTTP Status yang dihasilkan oleh method. 
    @Response Body : menyatakan bahwa apapun hasil yang dikembalikan oleh method, langsung dikirim ke requester. Hasil ini akan
    dikonversi sesuai format yang diminta requester melalui header 'Accept'. Misalnya bila requester mengirim header 'Accept:
    application/json', maka data akan dikirim dalam format JSON.
    @RestController : mirip dengan '@Controller', dengan tambahan: semua method otomatis diberikan '@ResponseBody'

<h3>Thymeleaf</h3>

Template Engine : Untuk memasang variabel dalam halaman HTML. Bisa juga untuk menghasilkan file text, xl, dsb yang ada templatenya dan ingin digabungkan dengan data.

Berbagai template engine di java:
* JSP : Java Server Pages. JSP berbeda dengan JSF
* Velocity 
* Freemarker
* Thymeleaf
* Jasper Report (Apache POI, iText)

Untuk menambahkan template Thymeleaf di spring-boot yang harus dilakukakan adalah mengimport atau memasukkan depedency ke dalam pom.xml

    <depedency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </depedency>

Menampilkan data menggunakan template thymeleaf
buat file html di template folder dan masukkan table html untuk menampilakan data,
setelah table kita bisa menampilkan data lewat

    <//tr th:each ="p : ${dafrarPeserta}">
        <//td th:each = "${p.nama}"> <//td>
        <//td th:each = "${p.email"> <//td>
        <//td th:each = "${p.tanggalLahir"> <//td>
        <//td> edit | lihat | hapus <///td>
    <///tr>
Full source code class Peserta here : <a href="http://localhost:4000/2017/07/07/Java%20Code/PesertaListHtml/" title="PesertaListHtml">List.html Java Code</a>


lalu buat file htmlController java di dalam controller folder, disinilah kita akan membuat function untuk button edit | lihat | hapus.

Contoh untuk lihat function

    @RequesMapping("peserta/list")
    public void daftarPeserta(Model m) {
        m.addAttribute("daftarPeserta", pd.findAll());
    }

Contoh untuk hapus function

    @RequestMapping("/delete")
    public String hapus(@requestParam("id") String id){
        pd.delete(id);
        return "redirect:list"; 
    }

Contoh untuk edit/add function

    @RequestMapping(value = "/peserta/form", method = RequestMethod.GET)
    public String tampilkanForm(@RequestParam(value = "id", required = false) String id,
            Model m) {

        Peserta peserta = new Peserta();
        m.addAttribute("peserta", peserta);

        if (id != null && !id.isEmpty()) {
            peserta = pd.findOne(id);
            if (peserta != null) {
                m.addAttribute("peserta", peserta);
                return "peserta/form";
            } else {
                return "peserta/form";
            }
        }
        
        return "peserta/form";
<a href="http://localhost:4000/2017/07/07/Java%20Code/PesertaHtmlController/" title="PesertaHtmlController">Peserta Html Controller Java Code</a>


dan jgn lupa juga untuk memberi link dari edit, hapus function
        <//a href = "form.html" th:href = "@{/peserta/form(id=${p.id})}"> edit </a>
        <//a href = "view.html" th:href = "@{/peserta/view(id=${p.id})}"> lihat </a>
        <//a href = "delete.html" th:href = "@{/peserta/form(id=${p.id})}"> hapus </a>

dan bagian akhir untuk file list.html akan menjadi seperti dibawah ini:

    <//body ng-app="aplikasiPelatihan">
        <//h1>Daftar Materi<//h1>
        <//div ng-controller="HaloController">

            Email Anda : <input type="text" ng-model="email" />
            <button ng-click="tambahEmail()">Tambahkan</button>
            <br/>

            Halo {{email}}

            <br/>
        
            Daftar Email: 
            <ul>
                <li ng-repeat="e in daftarEmail">
                    {{e}}
                    <button ng-click="hapusEmail(e)">Hapus</button>
                </li>
            </ul>
        </div>

        <div ng-controller="MateriController">
            
            <table border="1">
                <thead>
                    <tr>
                        <th>Kode</th>
                        <th>Nama</th>
                    </tr>

                </thead>
                <tbody>
                    <tr ng-repeat="m in dataMateri.content">
                        <td>{{m.kode}}</td>
                        <td>{{m.nama}}</td>
                        <td><button ng-click="hapusMateri(m)">hapus</button></td>
                    </tr>
                </tbody>
            </table>

            <table border="1">
                
                <tbody>

                    <tr>
                        <td>Kode</td>
                        <td>
                            <input type="text" ng-model="materi.kode" required ="true" />
                        </td>
                        <td></td>
                    </tr>

                    <tr>
                        <td>Nama</td>
                        <td>
                            <input type="text" ng-model="materi.nama" required="true" />
                        </td>
                        <td></td>
                    </tr>

                    <tr>
                        <td>&nbsp;</td>
                        <td>
                            <button ng-click="simpanMateri">Simpan</button>
                        </td>
                        <td>&nbsp;</td>
                    </tr>

                </tbody>
            </table>

            
        </div>
        <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
        <script src="/js/pelatihan.js"></script>
    </body>
<a href="http://localhost:4000/2017/07/07/Java%20Code/MateriListHtml/" title=" Materi List">Materi List.html Java Code</a>

<h3>Spring Security - Login</h3>

pertama tambahkan dependency security di pom.xml

    <depedency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </depedency>

dan jalankan project setiap penambahan dependency di pom.xml supaya langsung di tambahkan dependency nya ke project kita.

dan create package 'config' baru lalu buat file 'KonfigurasiSecurity' di dalam package folder tersebut

    @Configuration
    @EnableWebMvcSecurity
    public class KonfigurasiSecurity extends WebSecurityConfigurerAdapter {

        @Autowired
        public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
            auth
            .inMemoryAuthentication()
                .withUser("user").password("password").roles("USER");
            }
    }

Dan kemudian buat file security.sql di dalam folder yang sama di tempat peserta.sql, disini kita akan membuat table s_users, s_roles dan s_user_role didalam database pelatihan. table s_users yang berisi id, username, password dan active, id inilah yang kita gunakan apakah ia dapat mengakses data sebagai admin ataupun sebagai staff, dan table s_roles yang berisi id dan nama, untuk idnya sendiri apakah user ini admin ataupun sebagai staff. dan satu lagi table yang kita buat adalah s_user_role yang berisi isi id_user dan id_role jadi ditable ini akan ditampung data id dari dua table sebelumnya. dan jangan lupa untuk create table dan insert into kita bisa mengcreate table dan insert data dengan cara manual.
dan query untuk login :

        select username, password, active as enabled from s_users where username = 'username';

dan untuk mendapatkan role :

        select r.nama as authority
        from s_users u join as s_user_role ur on u.id = ur.id_user
        join s_roles r on ur.id_role = r.id 
        where u.username = 'username';

Full source code untuk Security.sql : <a href="http://localhost:4000/2017/07/07/Java%20Code/SecuritySql/" title="SecuritySql">Security.sql Java Code</a>

Setelah semuanya berjalan lancar kita tambahkan sedikit perubahan kedalam file KonfigurasiSecurity :
        private static final String SQL_LOGIN
        = "select username, password, active as enabled " + "from s_users where username = ?";
        private static final String SQL_PERMISSION
        = "select r.nama as authority"
        + "from s_users u join as s_user_role ur on u.id = ur.id_user"
        + "join s_roles r on ur.id_role = r.id"
        + "where u.username = 'username '";
Dan untuk lebih lengkapnya kita bisa lihat full source code untuk KonfigurasiSecurity : <a href="http://localhost:4000/2017/07/07/Java%20Code/KonfigurasiSecurity/" title="Konfigurasi Security">Konfigurasi Security Java Code</a>



Full source code untuk Login.html : <a href="http://localhost:4000/2017/07/07/Java%20Code/LoginHtml/" title="Login Html ">Login Html Java Code</a>

Full source code untuk PesertaDaoTest : <a href="http://localhost:4000/2017/07/07/Java%20Code/PesertaDaotest/" title="PesertaDaotest">PesertaDaoTest Java Code</a>

Full source code untuk Peserta Form.html : <a href="http://localhost:4000/2017/07/07/Java%20Code/PesertaFormHtml/" title="PesertaFormHtml">Form.html Java Code</a>

Full source code untuk Peserta.sql : <a href="http://localhost:4000/2017/07/07/Java%20Code/PesertaSql/" title="PesertaSql">Peserta.sql Java Code</a>

Full source code untuk pom.xml : <a href="http://localhost:4000/2017/07/07/Java%20Code/Pomxml/" title="Pomxml">Pom.xml Java Code</a>