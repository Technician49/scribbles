# SCRIBBLES - tapping into the communications
The notes are scribbles in no particular order.
We dug up the ATM cable outside the Bank office and found a serial line.. Turns out we can observe and interfere.  
We tapped the data line.    

Then we cross-checked with the documentation we 'obtained' .. during a.. well .. let's call that a site visit :-).   
The research group noticed that ALL commands that can be used are between brackets [  ]    
Everything outside the start marker [ and end marker ] is ignored.
Not all commands are embedded in all ATM models.. It sometimes gives a 'bad response' and echo what was typed between the markers as feedback.  

See the tips for copying (selecting text in Putty terminal window) and pasting (right click in Putty Window).  
One can also open Visual Studio and start a Terminal session with a nice send and receive box.   
If the FT232 USB device for reading Serial signals is not regognized on your PC, please visit this address for drivers:
https://www.ftdichip.com/Drivers/VCP.htm  

# Commands   
## [ListPrivateBankNr]
Seems to work in residential areas where the high value private banking customers live.
Sort of High Avalability option to cache this locally in the ATM, we can only assume that the ATM queries this list once in a while.
It also will show the balance of the accounts.

## [Deposit:accountnr:money]
So funny, you can actually make any deposit to any account, so cool. No integrity check seems to be in place.  
The ATM must be able to do so but I wonder if that can be faked or it is in a config database.. research needed.  

## [TriggerSirene]
Why use this one? Probably for engineers.    

## [RebootKeyLost:ATM123:X]
To break the crypto would be very time consuming.   
It uses post quantum crypto, HMAC and ephemeral ECC with SECP512 quadratation variant of CRYSTALS-KYBER (uh huh)..   
I read about another way in the ATM manual that was'found' during our team 'research' at the ATM factory.   
It is actualy a downgrade attack based on the possible event where the private key of the ATM would be lost.   
**ATM123** is to be replaced by ATM + number (so 123 would be the number as noted on the ATM)
Then **the X is the Modulo 9 of the number**. In the case of 123 it would be 6. 
(tip: https://www.calculatorsoup.com/calculators/math/modulo-calculator.php )  
Not really secure but perhaps invented for service engineers in the field without random generator.  
Without this, the plain text inserts will not work.  

## [Dispense:accountnr:money] or [Dispense:accountnr:money-hash]  
This one is so cool!! we probably can fake a message to have the ATM dispense actual money.  
There seems to be a limit up until 250,- (so 250.00) according to the documentation we  .. eeuh.. found.   

Any amount abover 250,- **requires a SHA1 calculation** over the fields in this exact form "accountnumber:amount" without the quotes.  
SHA1 calculator e.g. at https://emn178.github.io/online-tools/sha1.html  
Give the command like this:
> [Dispense:accountnr:money-hash]  

Account number must be a valid one. There must be a way to get those..  
The decimals in the money seems depicted as 123.50 so a dot '.' and not a comma ','  

## [SetDepositOn:ATM123:X]
This actually registers the ATM as the kind that also takes customer money to make a deposit in their account.   
Not very usefull.. but perhaps we can spoof messages here.   
ATM123 is to be replaced by ATM + number (so 123 would be the number)
Then the X is the Modulo 9 of the number. In the case of 123 it would be 6

## [SetDepositOff:ATM123:X]
Why use this one? Must standard software option for this particular ATM model.
