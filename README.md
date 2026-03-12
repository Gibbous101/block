# block

PRac 1
||Public Key Pvt Key||
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP


#Generate RSA key pair
key=RSA.generate(2048)
private_key=key
public_key=key.publickey()


#Create cipher objects
encryptor=PKCS1_OAEP.new(public_key)
decryptor=PKCS1_OAEP.new(private_key)


#Message to encrypt
message=b"Hello gang"


#Encrypt
encrypted=encryptor.encrypt(message)
print("Encrypted: ",encrypted)


#Decrypt
decrypted=decryptor.decrypt(encrypted)
print("Decrypted: ",decrypted.decode())

||Ledger||
import binascii
import Crypto
from Crypto.PublicKey import RSA
from Crypto.Signature import PKCS1_v1_5


class Client:
    def __init__(self):
        random= Crypto.Random.new().read
        self._private_key=RSA.generate(1024,random)
        self._public_key=self._private_key.publickey()
        self.signer=PKCS1_v1_5.new(self._private_key)
   
    @property
    def identity(self):
        return binascii.hexlify(
            self._public_key.exportKey(format="DER")
        ).decode("ascii")
TYIT=Client()
print("\nPublic Key: ",TYIT.identity)




Prac 2
formula
P1= Price on first day
Pn= Price on last day
Final change(%)=(Pn-P1)/P1 x 100
log= ln(pn/p1)

Prac3 
inv=float(input("Enter your investment: "))
btc_price=float(input("Enter the current price of 1 btc: "))

#calc
btc_amount=inv/btc_price
#output
print("You will get ", btc_amount)


Prac4
print("3_tyit\n")
print("The account is created")

def deposit(balance):
    amount = float(input("Enter the amount to be deposit: "))
    balance += amount
    print(f"The deposit is successful and the balance in the account is {balance}")
    return balance

def withdraw(balance):
    amount = float(input("Enter the amount to withdraw: "))
    if balance >= amount:
        balance -= amount
        print(f"The withdrawal is successful and the balance in the account is {balance}")
    else:
        print("Insufficient Balance")
    return balance

def enquiry(balance):
    print(f"Balance in the account is {balance}")
    return balance

balance = 0
balance = deposit(balance)
balance = withdraw(balance)
enquiry(balance)


Prac5 
masti


Prac 6
||token.sol||
pragma solidity ^0.8.31;


contract tytoken{
    string public name="tytoken";
    string public symbol="tyt";
    uint public totalSupply;
    uint public decimals=18;
    address public owner;
    mapping(address=>uint256) public balanceOf;


    constructor(uint256 initialSupply) {
        totalSupply=initialSupply;
        balanceOf[msg.sender]=initialSupply;
    }


    function transfer (address toWhom , uint256 value)public returns (bool success){
        require (balanceOf[msg.sender]>=value, "Insufficient tokens");
        balanceOf[msg.sender]-=value;
        balanceOf[toWhom]+=value;
        return true;
    }
}

Prac7
||ERC.sol||

//SPDX-License-Identifier:MIT
pragma solidity ^0.8.20;
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";


contract MyToken is ERC20{
    constructor() ERC20("MyToken", "MTK") {
        _mint(msg.sender, 1000000 * 10 ** decimals());
    }
}

||Stake.sol||

//SPDX-License-Identifier:MIT
pragma solidity ^0.8.20;
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";


contract SimpleStaking{
    IERC20 public token;
    mapping(address=>uint256) public stakes;


    constructor(address _token){
        token=IERC20(_token);
    }


    function stake(uint256 amount) external {
        require(amount>0,"Zero amount");
        token.transferFrom(msg.sender, address(this), amount);
        stakes[msg.sender]+=amount;
    }


    function unstake(uint256 amount) external {
        require(stakes[msg.sender]>=amount,"Not enough stake");
        stakes[msg.sender]-=amount;
        token.transfer(msg.sender,amount);
    }
}

Prac 8
||Attendance.sol||

// SPDX-License-Identifier: MIT 
pragma solidity ^0.8.9; 
contract Attendance { 
 mapping(uint256 => bool) public isPresent; 
 //Stores roll no presence 
 constructor(uint256[] memory rollno) { 
 for (uint256 i=0; i < rollno.length; i++) { 
 isPresent[rollno[i]] = true; 
 } 
 } 
 //check attendance 
 function attend(uint256 roll) public view returns (bool) { 
 return isPresent[roll]; 
 } 
}

||TransferEther.sol||

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract TransferEther {

    address payable public receiver;

    constructor(address payable _receiver) {
        receiver = _receiver;
    }

    function transferEther() public payable {
        receiver.transfer(msg.value);
    }
}

||Bank.sol||

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Bank {

    mapping(address => uint) public balances;

    // Deposit Ether
    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }

    // Withdraw Ether
    function withdraw(uint amount) public {
        require(balances[msg.sender] >= amount, "Insufficient balance");

        balances[msg.sender] -= amount;
        payable(msg.sender).transfer(amount);
    }

    // Check balance
    function checkBalance() public view returns(uint){
        return balances[msg.sender];
    }
}

Prac 9

||new folder terminal||

npm init -y   (used to install package ) 
npm install -g truffle 
npm install truffle lite-server 
npx truffle init 

||Add.sol in contracts||

// SPDX-License-Identifier:MIT 
pragma solidity^0.8.0; 

contract Add { 
    function add(uint a, uint b) public pure returns (uint) { 
        return a + b; 
    } 
}

||1_deploy.js in migrations||

const Add = artifacts.require("Add");
module.exports=function(deployer){
deployer.deploy(Add);
};

||truffle-config.json line 67 and line 109||

||terminal||
npx truffle compile  --all 
npx truffle migrate –-reset 

Prac 10
||new folder terminal||

npm init -y   (used to install package ) 
npm install -g truffle 
npm install truffle lite-server 
npx truffle init 

||SimpleStorage.sol in contracts||

// SPDX-License-Identifier: MIT 
pragma solidity ^0.8.0; 
contract SimpleStorage { 
    uint public value; 
 
    function setValue(uint _value) public { 
        value = _value; 
    } 
 
    function getValue() public view returns (uint) { 
        return value; 
    } 
} 

||2_deploy_contracts.js in migrations||

const SimpleStorage = artifacts.require("SimpleStorage"); 
module.exports = function (deployer) { 
  deployer.deploy(SimpleStorage); 
}; 


||terminal||
npx truffle compile  --all 
npx truffle migrate –-reset 

||index.html||

<!DOCTYPE html> 
<html> 
<head>
    <title>Simple DApp</title> 
    <script 
src="https://cdn.jsdelivr.net/npm/web3@1.7.0/dist/web3.min.js"></script> 
    </head> 
    <body> 
    <h2>Simple Storage DApp</h2> 
 
    <button id="connectBtn">Connect Wallet</button> 
 
    <br><br> 
 
    <input type="number" id="valueInput" placeholder="Enter value"> 
    <button id="setBtn">Set Value</button> 
 
    <br><br> 
 
    <button id="getBtn">Get Value</button> 
    <p id="output"></p> 
 
    <script> 
    let web3; 
    let contract; 
    let accounts; 
 
    const contractAddress = 
"0x946390AF804A9F32ff6D4A773e240D54fb583923"; 
 
    const abi = [ 
    { 
        "inputs":[{"internalType":"uint256","name":"_value","type":"uint256"}], 
        "name":"setValue", 
        "outputs":[], 
        "stateMutability":"nonpayable", 
        "type":"function" 
    }, 
    { 
        "inputs":[], 
        "name":"getValue", 
        "outputs":[{"internalType":"uint256","name":"","type":"uint256"}], 
        "stateMutability":"view",  
        "type":"function" 
    } 
    ]; 
 
    document.getElementById("connectBtn").onclick = async () => { 
    if (typeof window.ethereum !== "undefined") { 
        web3 = new Web3(window.ethereum); 
        await window.ethereum.request({ method: "eth_requestAccounts" }); 
        accounts = await web3.eth.getAccounts(); 
        contract = new web3.eth.Contract(abi, contractAddress); 
        alert("Wallet Connected"); 
    } else { 
        alert("MetaMask not detected"); 
    } 
    }; 
 
    document.getElementById("setBtn").onclick = async () => { 
    if (!contract) { 
        alert("Connect wallet first"); 
        return; 
    } 
    const val = document.getElementById("valueInput").value; 
    await contract.methods.setValue(val).send({ from: accounts[0] }); 
    }; 
 
    document.getElementById("getBtn").onclick = async () => { 
    if (!contract) { 
        alert("Connect wallet first"); 
        return; 
    } 
    const result = await contract.methods.getValue().call(); 
    document.getElementById("output").innerText = "Stored Value: " + result; 
    }; 
    </script> 
 
    </body> 
    </html> 

||cmd||
npx lite-server
