# Connettere e programmare un display OLED per il Raspberry Pi

## Prerequisiti
1. Preparare il raspberry come descritto [qui](https://gist.github.com/ginocic/6c1f6e845266ac262f8b532d7405ddc7)
2. Connettere il display al raspberry come descritto [qui](https://www.the-diy-life.com/connect-and-program-an-oled-stats-display-for-your-raspberry-pi/)

## Configurazione del Raspberry
```bash
sudo raspi-config
```
  * <kbd>Interface Options</kbd>
  * <kbd>I2C</kbd>
  * <kbd>Yes</kbd>
  * <kbd>Ok</kbd>
  * <kbd>Finish</kbd>

```bash
riavvia
```
## Installazione moduli e librerie necessarie
Dopo il riavvio, riconnettersi al raspberry ed eseguire i seguenti comandi
```bash
sudo apt-get install python3-pip python3-smbus i2c-tools
git clone https://github.com/adafruit/Adafruit_CircuitPython_SSD1306.git
sudo pip3 install adafruit-circuitpython-ssd1306
```

## Testare il collegamento del display e trovare l'indirizzo I2C
```bash
sudo i2cdetect -y 1
```
Se il risultato è qualcosa del genere
```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- 3c -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```
allora il display è collegato e, in questo caso, l'indirizzo I2C è ```0x3c```.
Se così non fosse, ricontrollare tutti i collegamenti e che L'I2C sia abilitato.

## Testare il funzionamento del display
```bash
cd Adafruit_CircuitPython_SSD1306
cd examples
python3 ssd1306_pillow_demo.py
```
Se il display mostra la scritta "Hello World!", allora tutto funziona.
Se così non fosse, ricontrollare tutti i passaggi precedenti

## Scaricare lo script e i font
```bash
mkdir -p "$HOME/.stats"
cd "$HOME/.stats"
git clone https://github.com/ginocic/RaspberryPi-Display-OLED "$HOME/.stats"
```

## Fase finale
Testare che tutto funzioni correttamente
```bash
cd "$HOME/.stats" && python3 stats.py
```

Settare l'esecuzione automatica dello script all'avvio del rpi
```bash
crontab -e
```
aggiungere la seguente linea di codice alla fine del file
```
@reboot cd "$HOME/.stats" && python3 stats.py &
```

Riavviare per testare il funzionamento
