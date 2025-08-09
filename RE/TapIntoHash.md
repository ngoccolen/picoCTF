# Mô tả thử thách
<img width="958" height="753" alt="image" src="https://github.com/user-attachments/assets/e0e0dc58-6623-4131-a1d5-7266140cedef" />
# Solution
Phân tích file block_chain.py
<pre lang="markdown"> 
import time
import base64
import hashlib
import sys
import secrets

#định nghĩa khối trong chuỗi blockchain
class Block:
    def __init__(self, index, previous_hash, timestamp, encoded_transactions, nonce):
        self.index = index #stt của khối trong chuỗi
        self.previous_hash = previous_hash #mã băm hash của khối trước đó
        self.timestamp = timestamp #Thời điểm khối được tạo
        self.encoded_transactions = encoded_transactions #Dữ liệu được mã hóa của các giao dịch trong khối này
        self.nonce = nonce

    def calculate_hash(self): #tính hash
        block_string = f"{self.index}{self.previous_hash}{self.timestamp}{self.encoded_transactions}{self.nonce}" #lấy tất cả thông tin của khối
        return hashlib.sha256(block_string.encode()).hexdigest() #dùng SHA-256 để hash chuỗi


def proof_of_work(previous_block, encoded_transactions): #Mô phỏng quá trình proof of work trong blockchain, hàm tạo ra 1 block mới dựa trên khối trước và dữ liệu giao dịch
    index = previous_block.index + 1 #thứ tự kế tiếp (khối mới)
    timestamp = int(time.time()) #Thời gian hiện tại
    nonce = 0

    block = Block(index, previous_block.calculate_hash(),
                  timestamp, encoded_transactions, nonce) #tạo 1 block mới

    while not is_valid_proof(block): #hàm ktr xem block có hợp lệ ko
        nonce += 1 #nếu chưa hợp lệ thì tăng nonce
        block.nonce = nonce

    return block


def is_valid_proof(block):
    guess_hash = block.calculate_hash()
    return guess_hash[:2] == "00" #ktra xem 2 ký tự đầu của chuỗi hash có = 00 ko

#Giải mã chuỗi base-64 -> chuyển về dạng byte gốc -> chuỗi vb dạng unicode
def decode_transactions(encoded_transactions):
    return base64.b64decode(encoded_transactions).decode('utf-8')


def get_all_blocks(blockchain):
    return blockchain


def blockchain_to_string(blockchain): #chuyển toàn bộ blockchain thành một chuỗi văn bản bằng cách nối các hash của từng block
    block_strings = [f"{block.calculate_hash()}" for block in blockchain]
    return '-'.join(block_strings)


def encrypt(plaintext, inner_txt, key): #mã hóa 1 chuỗi plaintext, chèn thêm inner_txt vào giữa chuỗi plaintext, sau đó mã hóa bằng khóa key
    midpoint = len(plaintext) // 2 #tính điểm giữa của chuỗi plaintext

    first_part = plaintext[:midpoint]
    second_part = plaintext[midpoint:]
    modified_plaintext = first_part + inner_txt + second_part #chèn inner_txt vào giữa chuỗi plaintext
    block_size = 16
    plaintext = pad(modified_plaintext, block_size)
    key_hash = hashlib.sha256(key).digest() #tính sha-256 của key, rồi lấy giá trị nhị phân

    ciphertext = b''

    for i in range(0, len(plaintext), block_size):
        block = plaintext[i:i + block_size]
        cipher_block = xor_bytes(block, key_hash) #phép xor giữa block và giá trị nhị phân của key
        ciphertext += cipher_block

    return ciphertext


def pad(data, block_size):
    padding_length = block_size - len(data) % block_size
    padding = bytes([padding_length] * padding_length) #thêm các byte pad để tổng chiều dài chia hết cho block_size (16)
    return data.encode() + padding


def xor_bytes(a, b): #XOR từng byte trong a và b
    return bytes(x ^ y for x, y in zip(a, b))


def generate_random_string(length):
    return secrets.token_hex(length // 2)


random_string = generate_random_string(64)


def main(token):
    key = bytes.fromhex(random_string) #chuyển đổi chuỗi hex thành dạng bytes dùng làm key

    print("Key:", key)

    genesis_block = Block(0, "0", int(time.time()), "EncodedGenesisBlock", 0)
    blockchain = [genesis_block] #tạo block

    for i in range(1, 5): #tạo thêm 4 block tiếp theo
        encoded_transactions = base64.b64encode( #tạo nội dung giao dịch -> base64 encode
            f"Transaction_{i}".encode()).decode('utf-8')
        new_block = proof_of_work(blockchain[-1], encoded_transactions)
        blockchain.append(new_block)

    all_blocks = get_all_blocks(blockchain)

    blockchain_string = blockchain_to_string(all_blocks) #lấy hash của từng block và nối thành chuỗi
    encrypted_blockchain = encrypt(blockchain_string, token, key) #chèn token vào giữa chuỗi blockchain và mã hóa bằng key

    print("Encrypted Blockchain:", encrypted_blockchain)


if __name__ == "__main__":
    text = sys.argv[1]
    main(text)
</pre>
Để ý hàm encrypt, ta thấy
1. Hàm chia plaintext làm 2 nửa
2. Chèn inner_text vào giữa
3. Chia dữ liệu thành các block 16 bytes rồi XOR với key_hash(mã hóa bằng SHA-256)
Khi chạy chương trình, ta được kết quả
<img width="1562" height="227" alt="image" src="https://github.com/user-attachments/assets/d659ddf1-a50a-4430-9504-70a0fcf6955b" />
Vậy ta đã có key và ciphertext, ta có thể giải mã được từng khối của blockchain đã mã hóa và trích được flag
Ta sẽ viết 1 script để giải m
<pre lang="markdown"> 
import hashlib
import re
 
block_size = 16
 
 
def xor_bytes(a, b):
    return bytes(x ^ y for x, y in zip(a, b))
 
 
def decrypt(ciphertext, key):
    plaintext = b''
    key_hash = hashlib.sha256(key).digest()
 
    for i in range(0, len(ciphertext), block_size):
        block = ciphertext[i:i + block_size]
        plain_block = xor_bytes(block, key_hash)
        plaintext += plain_block
 
    return plaintext
 
 
def main():
    with open("enc_flag", "r") as f:
        key, c = f.readlines()
 
    key = eval(key[5:])
    c = eval(c[22:])
 
    decrypted_blockchain = decrypt(c, key)
 
    flag = re.search(b'picoCTF{.*}', decrypted_blockchain)
    print(flag.group(0).decode("utf-8"))
 
 
if __name__ == "__main__":
    main()
</pre>
Chạy chương trình ta sẽ được flag
# picoCTF{block_3SRhViRbT1qcX_XUjM0r49cH_qCzmJZzBK_4989f9ea}


