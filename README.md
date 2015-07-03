## MAX7219 driver and 8x8 LED matrix display

This library intended to output text messages to 8x8 LED matrix display ([pdf reference](https://raw.github.com/d2r2/go-max7219/master/docs/LED8x8_1088AS.pdf)) via MAX7219 driver chip ([pdf reference](https://raw.github.com/d2r2/go-max7219/master/docs/MAX7219-MAX7221.pdf)):
![image](https://raw.github.com/d2r2/go-max7219/master/docs/Matrix MAX7219.JPG)

## Golang usage

```go
func main() {
	// create new LED matrix with number of cascaded devices is equal to 1
	mtx := max7219.NewMatrix(1)
	// open SPI device with spibus and spidev parameters equal to 0 and 0
	// set LED matrix brightness is equal to 7
	err := mtx.Open(0, 0, 7)
	if err != nil {
		log.Fatal(err)
	}
	defer mtx.Close()
	// output text message to LED matrix
	mtx.SlideMessage("Hello world!!! Hey Beavis, let's rock!",
		max7219.FontCP437, true, 50*time.Millisecond)
}
```

## Dependencies

Use package [github.com/fulr/spidev](http://github.com/fulr/spidev) to interact with max7219 chip via Linux SPI device.

## Getting help

GoDoc [documentation](http://godoc.org/github.com/d2r2/go-max7219/max7219)

## Installation

```bash
$ go get github.com/d2r2/go-max7219/max7219
```

## Quick Start

To output a single letter to LED matrix by specifing ascii code use OutputAsciiCode call:
```go
	// Output a sequence of ascii codes in a loop
	font = max7219.FontCP437
	for i := 0; i <= len(font.GetLetterPatterns()); i++ {
		mtx.OutputAsciiCode(0, font, i, true)
		time.Sleep(500 * time.Millisecond)
	}
```
To output a single national letter either unicode letter (rune) to LED matrix use OutputChar call:
```go
	// Output non-latin national letter (russian for example).
	// You must be sure, that your national letter will match code page of the font used.
	mtx.OutputChar(0, max7219.FontZXSpectrumRus, 'Я', true)
```

This functionality works not only with Raspberry PI, but with counterparts as well (tested with Raspberry PI and Banana PI). It will works with any Raspberry PI clone, which support Kernel SPI bus, but you should in advance make SPI bus device present in /dev/ list.

## FAQ

## Credits

This project is mainly a fork of respective functionality originally written by [Richard Hull](https://github.com/rm-hull) in python: <https://github.com/rm-hull/max7219>. Nevertheless it differs in some parts: refuse some functionality (works only with matrix led), include extra functionality (extra fonts, support of national languages).

## License

Go-max7219 is licensed inder MIT License.
