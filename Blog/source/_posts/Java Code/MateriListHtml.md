---
title: Materi List Html
tags:
    - Files
categories:
    - Source Code
commands: true
date: 2017-07-07 09:00:00
---

## Materi list.html Java Code files

    <!DOCTYPE html>
    <!--
    To change this license header, choose License Headers in Project Properties.
    To change this template file, choose Tools | Templates
    and open the template in the editor.
    -->
    <html>
        <head>
            <title>Daftar Materi</title>
            <meta charset="UTF-8"/>
            <meta name="viewport" content="width=device-width, initial-scale=1.0"/>

        </head>
        <body ng-app="aplikasiPelatihan">
            <h1>Daftar Materi</h1>
            <div ng-controller="HaloController">

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
    </html>
