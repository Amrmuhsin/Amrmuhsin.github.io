---
title: PesertaHtmlController
tags:
    - Files
categories:
    - Source Code
commands: true
date: 2017-07-07 09:00:00
---

## Peserta Html Controller Java Code files

    package com.muhsin.pelatihan.controller;

    import com.muhsin.pelatihan.dao.PesertaDao;
    import com.muhsin.pelatihan.entity.Peserta;
    import java.io.File;
    import java.text.SimpleDateFormat;
    import java.util.Date;
    import javax.servlet.http.HttpSession;
    import javax.validation.Valid;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.beans.propertyeditors.CustomDateEditor;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.validation.BindingResult;
    import org.springframework.web.bind.WebDataBinder;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.InitBinder;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestMethod;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.multipart.MultipartFile;

    @Controller
    public class PesertaHtmlController {

        @Autowired
        private PesertaDao pd;

        @InitBinder
        public void initBinder(WebDataBinder binder) {
            SimpleDateFormat dateFormat = new SimpleDateFormat("dd-MM-yyyy");
            dateFormat.setLenient(false);
            binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, true));
        }

        @RequestMapping("/peserta/list")
        public void daftarPeserta(Model m) {
            m.addAttribute("daftarPeserta", pd.findAll());
        }

        @RequestMapping("/peserta/delete")
        public String hapus(@RequestParam("id") String id) {
            pd.delete(id);
            return "redirect:peserta/list";
        }

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
        }

        @RequestMapping(value = "/peserta/form", method = RequestMethod.POST)
        public String prosesForm(@Valid Peserta p, BindingResult errors,
                MultipartFile foto, HttpSession session) throws Exception {
            if (errors.hasErrors()) {
                return "peserta/form";
            }
            pd.save(p);

            String namaFile = foto.getName();
            String jenisFile = foto.getContentType();
            String namaAsli = foto.getOriginalFilename();
            Long ukuran = foto.getSize();

            System.out.println("Nama file : " + namaFile);
            System.out.println("Jenis file : " + jenisFile);
            System.out.println("Nama asli : " + namaAsli);
            System.out.println("Ukuran : " + ukuran);

            String lokasiPath = "/upload";
            String lokasiTomcat = session.getServletContext().getRealPath(lokasiPath);
            System.out.println("Lokasi Tomcat dijalankan : " + lokasiTomcat);
            String lokasiTujuan = lokasiTomcat + File.separator;

            //kalo mau ganti folder buat nyimpan fotonya.
            //String homeFolder = System.getProperty("user.home");
            //String lokasiTujuan = homeFolder + File.separator +"tmp";
            File folderTujuan = new File(lokasiTujuan);
            if (!folderTujuan.exists()) {
                folderTujuan.mkdirs();
            }

            File tujuan = new File(lokasiTujuan + File.separator + namaAsli);

            foto.transferTo(tujuan);
            System.out.println("File sudah di Copy ke : " + tujuan.getAbsolutePath());

            return "redirect:/peserta/list";
        }
        
        @GetMapping("/peserta")
        public String peserta (){
            return "peserta";
        }

    }
