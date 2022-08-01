# AES CBC 模式加密

```go
func Encrypt(data string) string {

	key := []byte("password12345678") //key 的长度需要保证为16 | 24 | 32
	iv := []byte("1234567891111111")


	content := []byte(data)

	plaintext := PKCS7Padding(content)


	fmt.Println(plaintext)
	block, _ := aes.NewCipher(key)

	ciphertext := make([]byte, len(plaintext))

	fmt.Println(block)
	model := cipher.NewCBCEncrypter(block, iv)
	model.CryptBlocks(ciphertext, plaintext)
	encryptaestext := base64.StdEncoding.EncodeToString(ciphertext)
	fmt.Printf("ciphertext: %v\n", encryptaestext)

	return encryptaestext

}

func Decrypt(encryptData string)  {

	key := []byte("password12345678") //key 的长度需要保证为16 | 24 | 32
	iv := []byte("1234567891111111")

	newblock, _ := aes.NewCipher(key)

	decryptText, _ := base64.StdEncoding.DecodeString(encryptData)
	game := cipher.NewCBCDecrypter(newblock, iv)
	game.CryptBlocks(decryptText, decryptText)//后面的参数是数据源，这里用了相同的变量，输出会覆盖

	fmt.Printf("ciphertext: %s\n", string(PKCS7UnPadding(decryptText)))
}


func PKCS7Padding(ciphertext []byte) []byte {
	//如果加密数据刚好16位则补入16个16
	padding := aes.BlockSize - len(ciphertext)%aes.BlockSize
	fmt.Println("xx",[]byte{byte(padding)})
	padtext := bytes.Repeat([]byte{byte(padding)}, padding)
	fmt.Println(padtext)
	return append(ciphertext, padtext...)
}

func PKCS7UnPadding(plantText []byte) []byte {
	length := len(plantText)
	unpadding := int(plantText[length-1])
	return plantText[:(length - unpadding)]
}

func main(){
    eData := utils.Encrypt("abcdefg")

	utils.Decrypt(eData)
}
```

