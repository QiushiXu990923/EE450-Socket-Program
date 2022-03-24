# EE450-Socket-Program
Qiushi Xu
USC ID:5862459308

port_number_serverA_UDP 21308
port_number_serverB_UDP 22308
port_number_serverC_UDP 23308
port_number_serverM_UDP 24308
port_number_serverM_TCP_A 25308
port_number_serverM_TCP_B 26308

In my project, all the functions including optinal part are implemented. 
The project contains readme, makefile, and 6 cpp code files, which are serverM.cpp, serverA.cpp, serverB.cpp, serverC.cpp, clientA.cpp and clientB.cpp.
Running "make all" will compile all the files and generate 6 executable file, which are serverM, serverA, serverB, serverC, clientA and clientB. And each of them does what they should do mentioned in the material, which results in supporting all the functions including Check_Wallet, TXCOINS, TXLIST and Statistics.

The format of all the messages exchanged:

client send to serverM:
	Check_Wallet: "1 username"
	TXCOINS: "2 sender recver amount"
	TXLIST: "3"
	Stats: "4 username"
	
serverM send to backend server:
	Check this user's balance in your log: "1 username"
	Request for latest serial number: "2"
	write this new transaction log: "3 sender recver amount"
	Request for all transactions in your log: "4"
	Request for transactions related to this user in your log: "5 username"
	
backend server reply to serverM:
	Reply for checking user's balance:
		if no transaction record in my log: "!"
		else:
			total balance calculated from my record: "amount" (may be negative)
	Reply for serial number: "number"
	Reply for write new transaction: "Log Saved" (not necessary)
	Reply for all transactions:
		number of transactions: "n"
		n pieces of transactions: "serial_num sender recver amount"
	Reply for user related transactions:
		number of transactions: "n"
		n pieces of transactions: "serial_num sender recver amount"

serverM reply to client:
	reply for Check_Wallet:
		if user does not exist: "-1"
		else: "balance" (0<=balance)
	reply for TXCOINS: "2 sender recver amount"
		if sender does not exist: "-1"
		if sender's insufficient balance: "-2 balance" (current balance)
		if recver does not exist: "-3"
		if sender and recver does not exist: "-4"
		if success: "balance" (balance after transaction)
	reply for TXLIST: no reply
	reply for Stats: 
		if user does not exist: "-1"
		else: "1 other_user num_of_trans total_amount 2 other_user num_of_trans total_amount ... 0" (total amount can be negative)
	reply for invalid: "Invalid operation!"
	
I have to mention that in serverM, I used select() function which is partially adapted from Beej's Guide to Network Programming in order to process the incoming connection from clients. So it doesn't matter whether clientA or clientB run first.
