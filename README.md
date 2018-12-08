# KVAL-bbolt

[![Build Status](https://travis-ci.org/kval-access-language/kval-bbolt.svg?branch=master)](https://travis-ci.org/kval-access-language/kval-bbolt)
[![GoDoc](https://godoc.org/github.com/kval-access-language/kval-bbolt?status.svg)](https://godoc.org/github.com/kval-access-language/kval-bbolt)
[![Go Report Card](https://goreportcard.com/badge/github.com/kval-access-language/kval-bbolt)](https://goreportcard.com/report/github.com/kval-access-language/kval-bbolt)

[**bbolt**](https://github.com/etcd-io/bbolt) bindings for
[KVAL](https://github.com/kval-access-language/kval)

Package `kvalbbolt` implements a bbolt binding for KVAL (Key Value Access
Language). The binding provides more than just 'boilerplate' for working with
bbolt. It implements a language thus enabling access at a higher abstraction.
Users of the binding can provide simple instructions and the binding will take
care of Bucket creation (and deletion), and the generation of keys and values,
plus their maintenance, no matter how deep into the database structure they are
needed, or indeed exist.

*"Everything should be made as simple as possible, but no simpler." - Albert
Einstein*

The most up-to-date specification for KVAL can be found here:
https://github.com/kval-access-language/kval

My first blog post describing my thoughts a little better can be found here:
http://exponentialdecay.co.uk/blog/key-value-access-language-kval-for-boltdb-and-golang/

### Key Value Access Language

I have created a modest specification for a key value access language.
It allows for input and access of values to a key value store such as Golang's
[bbolt](https://github.com/etcd-io/bbolt).

The language specification: https://github.com/kval-access-language/KVAL

### Features

* Single function entry-point:

    * `res, err := Query(kb, "INS B1 >> B2 >> B3 >>>> KEY :: VAL") // Will create three buckets, plus k/v in one-go`
    * `res, err := Query(kb, "GET B1 >> B2 >> B3 >>>> KEY") // Will retrieve that entry in one-go`

* Start using bbolt immediately without writing boiler plate before you can
code.
* KVAL-Parse enables handling of Base64 encoded binary BLOBS.
* Regular Expression based searching for key names and values/
* [KVAL Language](https://github.com/kval-access-language/KVAL) specifies easy
bulk or singleton DELETE (`DEL`) and RENAME (`REN`) capabilities.
* It's a language specification so other bindings for other databases are
possible.

### Usage

Use of the `kvalbbolt` binding is simple. There is one function which accepts a
string formatted to KVAL's specification:

    res, err = Query(kb, "GET Bucket One >> Bucket Two >>>> Requested Key")
    if err != nil {
       fmt.Fprintf(os.Stderr, "Error querying db: %v", err)
    } else {
       //Access our result structure
       res.Result // a map[string]string
    }

For write operations we simply check for the existence of an error, else the
operation passed as expected:

    res, err = Query(kb, "INS Bucket One >> Bucket Two >>>> Insert Key :: Insert Value")
    if err != nil {
       fmt.Fprintf(os.Stderr, "Error querying db: %v", err)
    }

### How easy is it?

Once you've a connection to a database, call `Query(...)` as many times as you
like to work with your data. The most basic implementation, creating a DB, and
inserting data looks as follows:

	package main

	import (
		"fmt"
		"os"
		kval "github.com/kval-access-language/kval-bbolt"
	)

	func main() {

		kb, err := kval.Connect("newdb.bolt")
		if err != nil {
			fmt.Fprintf(os.Stderr, "Error opening bolt database: %#v", err)
			os.Exit(1)
		}
		defer kval.Disconnect(kb)

		//Lets do a test insert...
		res, err := kval.Query(kb, "INS test bucket one >> test bucket two >>>> key one :: value one")
		if err != nil {
			//work with your error
		}
		// else: start working with you res struct
	}

### Demo

Have a look at some of the bits and pieces implemented as part of this binding
in the demo Go application here:
https://github.com/kval-access-language/kval-boltdb-demo

[![asciicast](https://asciinema.org/a/215878.svg)](https://asciinema.org/a/215878)

### How to contribute

The following would be helpful:

* Comments on the KVAL specification, working towards a version 1.
* Code review
* Testers - Get using it and report your issues!
* Spread the word
* Let me know how it goes here via [Issue](https://github.com/kval-access-language/kval-bbolt/issues),
or on Twitter via [@beet_keeper](https://twitter.com/beet_keeper)

### License

**[GPL Version 3](http://choosealicense.com/licenses/gpl-3.0/)**:
https://github.com/kval-access-language/KVAL-BBolt/blob/master/LICENSE
