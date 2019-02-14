# HyperledgerBlockchainTrainingLabs
Hyperledger Activity

Blockchain training lab

1. clone this repo: https://github.com/svapolonio/HyperledgerBlockchainTrainingLabs

2. copy chaincode and supply folder from blockchain-training-labs and paste it from the  previous project fabric-samples folder. *merge these files.

3. Open terminal and go to supply folder inside fabric-samples ( ~/fabric-samples/supply$ )

4. Download the requried library for our chaincode, Go language must be installed

Run this on your terminal: 

go get github.com/golang/protobuf/proto
go get github.com/hyperledger/fabric/common/attrmgr
go get github.com/pkg/errors
go get github.com/hyperledger/fabric/core/chaincode/lib/cid

5. Open your file manager go to Home/go/src/github.com
	copy three folders and paste it inside fabric-samples/chaincode:
		hyperledger
		pkg
		golang

6. Start the fabric and you should see following:

	~/fabric-samples/supply$ ./startFabric.sh
	~/fabric-samples/supply$ npm install	
	~/fabric-samples/supply$ node enrollAdmin.js
	~/fabric-samples/supply$ node registerSupplier.js
	~/fabric-samples/supply$ node registerOEM.js
	~/fabric-samples/supply$ node registerBank.js
	~/fabric-samples/supply$ node app.js
	

7. Open postman, change method from GET to POST and add url localhost:3000/invoice.
Below url should see Params Authorization Headers Body, click Headers add another key below Content-Type, type user and value ‘Supplier’
Now open the body tab, click the x-www-form-url-encoded, you should see a form there type;

KEY                      	VALUE

invoicenumber           INVOICE001
billedto                	OEM
invoicedate             	02/08/19
invoiceamount          	10000
itemdescription         	KEYBOARD
goodreceived            	False
ispaid                  	False
paidamount              	0
repaid                 	False
repaymentamount       0

Now click send, the result should be ‘success’
We have now successfully raise an invoice

Add another request GET method localhost:3000, on header add user with value of supplier then click SEND. Results will show below.


8. Declare goodreceived
	Add another request PUT method localhost:300/invoice, click header tab below then add user with value of oem. Next open the body tab x-www-form-url-encoded and type these data;

invoicenumber	  INVOICE0001
goodreceived		True

Now click send, the result should be ‘success’


9. Bank pays the supplier
	Add another request PUT method localhost:3000/invoice, click header tab below then add user  
with the value of bank. Next open the body tab x-www-form-url-encoded and type these data;

invoicenumber	  INVOICE0001
paidamount		  9000

*The paid amount should be less than invoice amount

Now click send, the result should be ‘success’

*Go back to GET method tab then hit send and check if data is updated. The invoice will indicate that the isPaid = true and the paidamount will be 9000


10. OEM pay the bank
	Add another request PUT method localhost:3000/invoice, click header tab below then add user  with the value of bank. Next open the body tab x-www-form-url-encoded and type these data;

invoicenumber		  INVOICE0001
repaymentamount		11000

*The repayment amount should be more than paidamount

Now click send, the result should be ‘success’

*Go back to GET method tab then hit send and check if data is updated.


11. Check invoice audit trail
	Add another request GET method localhost:3000, on header tab add user with value of supplier, Next open the body tab x-www-form-url-encoded and type these data;

invoicenumber		INVOICE001

Now click send, you should see the respond from the server change it from Html to Json format of the response 
