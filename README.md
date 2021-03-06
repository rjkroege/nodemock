Node Mock - Simple Yet Powerful Mocking Framework for NodeJs
============================================================

NodeMock is a very simple to use mocking framework which can be used to 
mock functions in JavaScript objects. 
NodeMock creates mock methods in less code with more expressive manner

Features
--------
Besides it's simplicity it supports following features

* Does not need an existing object to create the mock
* Verify arguments (we check deeply on objects and arrays to check the validity)
* Allow a return to be sent
* Assertion to check whether all the rules executed
* Callbacks can also be executed with providing arguments
* Multiple mock functions in one object
* Alter a mock function later on
* Method chaining allows creating mocks super easy
* Fail support added when calling method that should not be called
* Mock support to call a single method more than once
* Repetitive support
	
Testing
-------
Node JS can be used with any testing framework. And we've used it with 
Nodeunit and it's a perfect match.
(https://github.com/arunoda/nodemock/blob/master/test/nodemock.js "Nodemock with Nodeunit")

Install
---------
npm install nodemock

Usage
------

### Load the Module
	var nodemock = require("nodemock");

### Creating a mock function with taking arguments and return value
	var mocked = nodemock.mock("foo").takes(10, [10, 20, 30]).returns(98);
	
	mocked.foo(10, [10, 20, 30]); // this will return 98
	
	mocked.foo(10); //throws execption
	
### Creating a mock with callback support
	var mocked = nodemock.mock("foo").takes(20, function(){}).calls(1, [30, 40]);
	
	mockes.foo(20, function(num, arr) {
		console.log(num); //prints 30
		console.log(arr); //prints 40
	});
	
	/*
		When you invoke foo() nodemock will calls the callback(sits in argument index 1 - as specified)
		with the parameters 30 and 40 respectively. 
	*/
	
### Add multiple mock functions
	var mocked = nodemock.mock("foo").takes(10).returns(30);
	mocked.foo(10); //gives 30
	
	mocked.mock("bar").takes(true).returns(40);
	mocked.bar(true); // gives 40
	
### Assertion Support
	var mocked = nodemock.mock("foo").takes(20);
	var mocked = nodemock.mock("bar").takes(40);
	
	mocked.foo(20);
	mocked.bar(40);
	
	//check whether what we've defined is actually executed
	mocked.assert(); //returns true
	
### Fails when calls any method in the mock object
	var mocked = nodemock.fail();
	mocked.foo(); //thorws an exception
	mocked.bar(); //throws an exception
	
### Fails when calls some particular method in the mock object
	var mocked = nodemock.mock("foo").fail();
	mocked.mock("bar").takes(10);
	mocked.foo(); //thorws an exception
	mocked.bar(10); //works perfectly
	
### calls a single mocked method, multiple times
	
	var mocked = nodemock.mock("foo").takes(10, 20).times(2);
	
	mocked.foo(10, 20);
	mocked.foo(10, 20);
	
### mock a single method more than once
	var mocked = nodemock.mock("foo").takes(10, 20);
	mocked.mock("foo").takes(20, 30);
	mocked.mock("foo").takes(500);
	
	mocked.foo(10, 20);
	mocked.foo(20, 30)
	mocked.foo(500);
	
	//check whether everything has done
	mocked.assert(); //returns true
	
	
API Documentation
-----------------

### Construction ###

	var mocked = require('nodemock').mock('foo');
		Creating a object with mock function "foo"
	
	mocked.mock(methodName)
		Used to alter or create a new mock method and add rules to it as usual
		
### Rules ###

	mocked.takes(arg1, args2, ...)
		Specify arguments of the function and verify then when calling
		
	mocked.returns(returnValue)
		Specify the return value of the function
		
	mocked.calls(callbackPosition, argumentsArray)		 
		Calls a callback at the arguments in index `callbackPosition`
		with the arguments specified in the "argumentsArray"
		
		when using this you've to define a function signature as a callback in the argument list
		for a callback at index 2 .takes() function will be as,
		
		`mocked.takes(10, 20, function(){})
	
		
	mocked.fail()
		If calls at very begining afterword any call on the mocked objects will fail
		Otherwise current mock method will fails someone called that. 
		
	mocked.times(repetitiveCount);
		We can rule the mocked method to be called multiple times with same parameters
		Finally we can check that using above assert method;
	
### Confirm ###

	mocked.assert();
		Checks whether rules we've defined using other methods were executed.
		If all the rules were executed return true, otherwise false
		
License
-------
The MIT License

Copyright (c) 2011 Arunoda Susiripala

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

