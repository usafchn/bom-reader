This project is forked from [dimchansky/utfbom](https://github.com/dimchansky/utfbom). And the main
change of this project is to support GB-18030 encoding, see (
wikipedia)[https://en.wikipedia.org/wiki/Byte_order_mark] for details

Below is the original README information

---

The package utfbom implements the detection of the BOM (Unicode Byte Order Mark) and removing as
necessary. It can also return the encoding detected by the BOM.

## Installation

    go get -u github.com/dimchansky/utfbom
    
## Example

```go
package main

import (
	"bytes"
	"fmt"
	"io/ioutil"

	"github.com/dimchansky/utfbom"
)

func main() {
	trySkip([]byte("\xEF\xBB\xBFhello"))
	trySkip([]byte("hello"))
}

func trySkip(byteData []byte) {
	fmt.Println("Input:", byteData)

	// just skip BOM
	output, err := ioutil.ReadAll(utfbom.SkipOnly(bytes.NewReader(byteData)))
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("ReadAll with BOM skipping", output)

	// skip BOM and detect encoding
	sr, enc := utfbom.Skip(bytes.NewReader(byteData))
	fmt.Printf("Detected encoding: %s\n", enc)
	output, err = ioutil.ReadAll(sr)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("ReadAll with BOM detection and skipping", output)
	fmt.Println()
}
```

Output:

```
$ go run main.go
Input: [239 187 191 104 101 108 108 111]
ReadAll with BOM skipping [104 101 108 108 111]
Detected encoding: UTF8
ReadAll with BOM detection and skipping [104 101 108 108 111]

Input: [104 101 108 108 111]
ReadAll with BOM skipping [104 101 108 108 111]
Detected encoding: Unknown
ReadAll with BOM detection and skipping [104 101 108 108 111]
```


