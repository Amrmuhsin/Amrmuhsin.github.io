---
title: Peserta Dao
tags:
    - Files
categories:
    - Source Code
commands: true
date: 2017-07-07 09:00:00
---

## Peserta Dao Java Code files

	package com.muhsin.pelatihan.dao;

	import com.muhsin.pelatihan.entity.Peserta;
	import org.springframework.data.repository.PagingAndSortingRepository;

	public interface PesertaDao extends PagingAndSortingRepository<Peserta, String > {
	}