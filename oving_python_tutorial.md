# CanSat Øvinger Tutorial (Python)

### @diffs true
### @unifiedToolbox true

<!-- Del 1: -->

## CanSat Øvingsoppgaver @unplugged

I denne veiledningen skal vi gå gjennom grunnleggende funksjoner som dere får bruk for når dere skal programmere en CanSat. 

**Oppgaver dere skal løse er: **

**1)** Få et LED-lys til å blinke ved koble det til pins på micro:bit

**2)** Lage en teller som teller fra 10 til 0, og så skrur på LED-lyset i 5 sekunder.

**3)** Bruke en OLED-skjerm, og vise nedtellingen på skjermen.

**4)** Lage et voltmeter som skriver spenningen til OLED-skjermen.

**5)** Lese analogverdi fra en TMP36 (Temperatursensor) og konvertere disse til temperatur vi viser på OLED-skjermen.

**6)** Koble til en BMP280 sensor, les av lufttrykk og skriv det på OLED-skjerm.

**7)** Regne om barometrisk lufttrykk til relativ høyde på CanSat.

**8)** Sende data mellom 2 micro:bit og lagre de i en datalogger

#### **Lykke til!**


<!-- Del 1.1: -->

## Oppgave 1 - Koble LED-lys til CanSat @unplugged

Koble opp kretsen som vist på bildet under.

NB: Koble pluss på LED til inngangen P0 og minus til jord (GND).

![microbit-ovelse-1-LED.jpg](https://i.postimg.cc/wBXCyNSs/microbit-ovelse-1-LED.jpg)


<!-- Del 1.2: -->

## Oppgave 1: Få LED-lyset til å skru seg AV og PÅ én gang hvert sekund.

Inni ``||basic: gjenta for alltid||``:

Finn blokken ``||pins: skriv digital til||`` som vi skal bruke for å skru AV og PÅ LED-lyset vi har koblet til CanSat. Sett verdien til 1 for å skru lyset PÅ og 0 for å skru det AV.

Bruk en ``||basic: pause||`` for å si hvor lenge lyset skal være PÅ og AV.

**Ekstra**: Endre hvor fort LED-lyset blinker ved å justere på tiden i ``||basic: pause||``-blokken.

**HINT**: 1000 ms i ett sekund


```python
def on_forever():
    pins.digital_write_pin(DigitalPin.P0, 1)
    basic.pause(500)
    pins.digital_write_pin(DigitalPin.P0, 0)
    basic.pause(500)
basic.forever(on_forever)
```

<!-- Del 2.1: -->

## Oppgave 2: Telle fra 10 til 0

**Når man skal lage store koder, er det vanlig å bruke ``||functions: funksjoner||``. Inni funksjonen bygger vi koden for det vi vil den skal utføre, og så kaller vi den opp når vi trenger den.**

Start med å lage funksjonen ``||functions: nedtelling||``:

Lag en ny variabel: ``||variables: teller ||``. Sett den til som en global variabel inni ``||functions: nedtelling||``. Sett ||variables: teller = 10 ||``.

Siden vi skal telle ned fra 10 til 0, bruker vi en ``||loops: FOR-løkke ||`` som lar oss gjenta løkken akkurat så mange ganger vi ønsker. 

Sett ``||loops: for indeks in range (11) ||``. Inni her skal vi vise ``||variables: teller ||`` og så endre den med -1.

Kjør ``||functions: nedtelling||`` når micro:bit starter.

```python
def nedtelling():
    global teller
    teller = 10
    for indeks in range(11):
        basic.show_number(teller)
        indeks += -1
teller = 0
nedtelling()
```

<!-- Del 2.2: -->

## Oppgave 2: Få LED-lys til å lyse i 5 sek.

Flytt 

    pins.digital_write_pin(DigitalPin.P0, 1) 
    basic.pause(500)

over til ``||functions: nedtelling ||``. 

Endre ``||basic: pause ||``  mellom LED PÅ og AV til 5000 ms. 


```python
def nedtelling():
    global teller
    teller = 10
    for indeks in range(11):
        basic.show_number(teller)
        indeks += -1
        pins.digital_write_pin(DigitalPin.P0, 1)
        basic.pause(5000)
        pins.digital_write_pin(DigitalPin.P0, 0)
teller = 0
nedtelling()
```

<!-- Del 2.3: -->

## Oppgave 2: Bestemme når nedtelling skal starte

``||functions: Funksjonen nedtelling||`` kjøres når vi starter micro:biten. Hvis vi vil at den skal kjøres på nytt, kan vi f.eks. kalle den opp når vi ``||input: trykker på knapp A||``.



```python
def nedtelling():
    global teller
    teller = 10
    for indeks in range(11):
        basic.show_number(teller)
        indeks += -1
        pins.digital_write_pin(DigitalPin.P0, 1)
        basic.pause(5000)
        pins.digital_write_pin(DigitalPin.P0, 0)
teller = 0
def on_button_pressed_a():
    nedtelling()
input.on_button_pressed(Button.A, on_button_pressed_a))
```

<!-- Del 3: -->

## Oppgave 3: Skriv til OLED-skjerm @unplugged

Nå skal vi se på hvordan vi kan bruke en Kitronik OLED-skjerm for å bedre vise dataene våre. 

Skjermen skal kobles til mellom CanSat og micro:bit.

![Kitronik OLED-skjerm](https://www.lekolar.no/globalassets/inriver/resources/133848_118895.jpg)


<!-- Del 3.1: -->

## Oppgave 3: Sette opp OLED-skjerm

Fra biblioteket ``||kitronik_VIEW128x64: 128x64 Display||``, hent kommandoene ``||kitronik_VIEW128x64: turn displayOutput display ||`` og ``||kitronik_VIEW128x64: Set font size to||``, og la de kjøres når microbiten skrus på.

Sett display til ``||kitronik_VIEW128x64: True||`` og sett font til ``||kitronik_VIEW128x64: BIG||``.

```python
kitronik_VIEW128x64.control_display_on_off(True)
kitronik_VIEW128x64.set_font_size(kitronik_VIEW128x64.FontSelection.BIG)
```


<!-- Del 3.2: -->

## Oppgave 3: Vise nedtelling på OLED-skjermen

Fjern ``||basic: basic.show_number||`` ``||variables: (teller)||`` fra ``||functions: def nedtelling||``. 

Sett istedet inn ``||kitronik_VIEW128x64: clear display||`` og ``||kitronik_VIEW128x64: show inputData||``. Endre (None) til (``||variables: teller||``, ``||kitronik_VIEW128x64: 1||``). *Tallet etter kommategnet sier hvilken linje som kommandoen skal skrives på.*

```python
def nedtelling():
    global teller
    teller = 10
    for indeks in range(11):
        kitronik_VIEW128x64.clear()
        kitronik_VIEW128x64.show(teller, 1)
        indeks += -1
        pins.digital_write_pin(DigitalPin.P0, 1)
        basic.pause(5000)
        pins.digital_write_pin(DigitalPin.P0, 0)

def on_button_pressed_a():
    nedtelling()
input.on_button_pressed(Button.A, on_button_pressed_a)

teller = 0
kitronik_VIEW128x64.control_display_on_off(True)
kitronik_VIEW128x64.set_font_size(kitronik_VIEW128x64.FontSelection.BIG)
```


<!-- Del 4: -->

## Oppgave 4: Lage et voltmeter @unplugged

Alle sensorene vi skal koble til CanSat'en bruker spenningsverdien vi får fra sensoren for å regne ut den faktiske verdien vår. Vi må derfor lære hvordan man konverterer den analog verdi vi får inn på micro:biten til en spenningsverdien.

**Fjern hele funksjonen ``||functions: def nedtelling||``**


<!-- Del 4.1: -->

## Oppgave 4: Lagre analog verdi på micro:biten

Start med å lage en ny funksjon: ``||functions: voltmeter||``.

Lage en ny variabel: ``||variables: analogVerdi||``. *Husk å gjøre variabelen global inni ``||functions: def voltmeter||``*

Inni ``||functions: def voltmeter||``, sett ``||variables: analogVerdi||`` lik ``||pins: lese analogverdi fra AnalogPin.P0||``.

Husk å kjøre voltmeter() i ``||basic: gjenta for alltid||``.

```python
def on_forever():
    voltmeter()
basic.forever(on_forever)

analogVerdi = 0
def voltmeter():
    global analogVerdi
    analogVerdi = pins.analog_read_pin(AnalogPin.P0)
```

<!-- Del 4.2: -->

## Oppgave 4: Vise analog verdi på OLED-skjermen

Sett inn ``||kitronik_VIEW128x64: clear display||`` inni ``||basic: gjenta for alltid||``.  Sett ``||kitronik_VIEW128x64: show inputData||`` inni ``||functions: def voltmeter||``. Endre (None) til ``||kitronik_VIEW128x64: (0, 1)||``. *Bytt tallet før kommategnet (0) med hva som skal vises på skjermen, og tallet etter hvilken linje som kommandoen skal skrives på.*

For å kunne vise både tekst og en variabel på samme linje, må vi bruke ``||text: concat ||``. Se eksempelet under for hvordan det skal gjøres.

    kitronik_VIEW128x64.show("Verdi jeg vil vise: " + str(analogVerdi) + " Enhet", 1)

Til slutt, oppdater verdien hvert 500ms: Bruk en ``||basic: pause||`` for å gjøre dette.

```python
def on_forever():
    kitronik_VIEW128x64.clear()
    voltmeter()
basic.forever(on_forever)

analogVerdi = 0
def voltmeter():
    global analogVerdi
    analogVerdi = pins.analog_read_pin(AnalogPin.P0)
    kitronik_VIEW128x64.show("Analog: " + str(analogVerdi), 1)
    basic.pause(500)
```


<!-- Del 4.3: -->

## Oppgave 4: Bestemme verdi på Uref

Lage en ny variabel: ``||variables: Uref||``.

``||variables: Uref||`` skal bstemmes når microbiten skrus på. Verdien vi skriver inn her er avhengig av om CanSat får strøm fra USB eller batteri.

Se tabell under for hva ``||variables: Uref||`` skal være :

|   **  Spenning fra USB  **   |   |   **  Spenning fra batteri  **   |
| :------------: | :------------: |:------------: |
| 3.2 |  | 3.0 |

```python
Uref = 3.2
```


<!-- Del 4.4: -->

## Oppgave 4: Beregne spenning fra analog verdi

Lage en ny variabel: ``||variables: spenning||``.

Bruk denne formelen for å regne ut ``||variables: spenning||``:

![Formel-spenning-fra-analogverdi.png](https://i.postimg.cc/v8121cD6/Formel-spenning-fra-analogverdi.png)

```python
analogVerdi = 0
spenning = 0
Uref = 3.2
def voltmeter():
    global analogVerdi
    global spenning
    global Uref
    analogVerdi = pins.analog_read_pin(AnalogPin.P0)
    spenning = (analogVerdi / 1024) * Uref 
    kitronik_VIEW128x64.show("Analog: " + str(analogVerdi), 1)
    basic.pause(500)
```


<!-- Del 4.5: -->

## Oppgave 4: Vis spenningsverdi på OLED-skjerm

Vi skal gjøre det samme som vi gjorde for å vise den analoge verdien på OLED-skjermen:

    kitronik_VIEW128x64.show("Spenning: " + str(spenning) + " V", 2)

**Bonus:** Endre skriftstørrelse på OLED-skjerm til ``||kitronik_VIEW128x64: Normal ||``.

```python
analogVerdi = 0
spenning = 0
Uref = 3.2
def voltmeter():
    global analogVerdi
    global spenning
    global Uref
    analogVerdi = pins.analog_read_pin(AnalogPin.P0)
    spenning = (analogVerdi / 1024) * Uref 
    kitronik_VIEW128x64.show("Analog: " + str(analogVerdi), 1)
    kitronik_VIEW128x64.show("Spenning: " + str(spenning) + " V", 2)
    basic.pause(500)
```


<!-- Del 4.6: -->

## Oppgave 4: Runde av spenningsverdi

Når du testet koden din, så du kanskje at du fikk veldig mange desimaler. Vi kan ikke direkte skrive at vi kun ønsker 2 desimaler. 

Det vi må gjøre er å gange verdien vi har fått med 100, ``||math: avrund ||`` den nye verdien vår, før vi deler den på 100.

    spenning = int(analogVerdi / 1024 * Uref * 100) / 100

```python
analogVerdi = 0
spenning = 0
Uref = 3.2
def voltmeter():
    global analogVerdi
    global spenning
    global Uref
    analogVerdi = pins.analog_read_pin(AnalogPin.P0)
    spenning = int(analogVerdi / 1024 * Uref * 100) / 100 
    kitronik_VIEW128x64.show("Analog: " + str(analogVerdi), 1)
    kitronik_VIEW128x64.show("Spenning: " + str(spenning) + " V", 2)
    basic.pause(500)
```


<!-- Del 5: -->

## Oppgave 5: Lage et termometer med TMP36 @unplugged

I denne oppgaven skal vi finne temperaturen fra en TMP36 temperatursensor. 

Koble opp kretsen på bildet under:

![Oppgave-5-TMP36-Oppkobling.png](https://i.postimg.cc/g2RTLYvL/Oppgave-5-TMP36-Oppkobling.png)


<!-- Del 5.1: -->

## Oppgave 5: Regne om spenningsverdi til temperatur

Vi skal lage en ny funksjon: ``||functions: termometer||``.

Inne ``||functions: def termometer||``, lage en ny global variabel: ``||variables: temperatur||``.

Bruk denne formelen for å beregne ``||variables: temperatur||``:

![Formel-temperatur-fra-spenning.png](https://i.postimg.cc/wM89Cn7m/Formel-temperatur-fra-spenning.png)

*Husk å kjøre ``||functions: termometer||`` i ``||basic: gjenta for alltid||``*

```python
def on_forever():
    termometer()
basic.forever(on_forever)

def termometer():
    global spenning
    global temperatur
    temperatur = (spenning - 0.5) - 0.01
```

<!-- Del 5.2: -->

## Oppgave 5: Vise termometer på OLED-skjerm

Vi skal gjøre det samme som vi gjorde for å vise den analoge verdien på OLED-skjermen:

    kitronik_VIEW128x64.clear()
    kitronik_VIEW128x64.show("Temperatur: " + str(temperatur) + " C", 3)


```python
def termometer():
    global spenning
    global temperatur
    temperatur = (spenning - 0.5) - 0.01
    kitronik_VIEW128x64.show("Temperatur: " + temperatur + " C", 3)
```

<!-- Del 6: -->

## Oppgave 6: Lag et barometer @unplugged

#### Vi skal bruke en ``||BMP280: BME280||`` sensor. 

![BME280.png](https://i.postimg.cc/ZYjVtVRH/BME280.png)

Denne sensoren kan måle:

- Temperatur
- Trykk
- Luftfuktighet
- Duggpunkt

**For å lage et barometer skal vi bruke ``||BMP280: trykk||``.**


<!-- Del 6.1: -->

## Oppgave 6: Koble opp og lese av fra BMP280

For å få BMP280 til å snakke med CanSat, skal vi bruke de to linjene under. De skal kjøres når micro:bit starter opp:

    BME280.power_on()
    BME280.address(BME280_I2C_ADDRESS.ADDR_0X76)

Videre, lage en ny funksjon: ``||functions: BMP280_sensor||``.

Inne ``||functions: BMP280_sensor||``, sett opp en ny global variabel: ``||variables: trykk||``. Sett ``||variables: trykk||`` til ``||BMP280: pressure||``.

    trykk = BME280.pressure(BME280_P.PA)

*Husk å kjøre ``||functions: BMP280_sensor||`` i ``||basic: gjenta for alltid||``*

```python
def on_forever():
    BMP280_sensor()
basic.forever(on_forever)

BME280.power_on()
BME280.address(BME280_I2C_ADDRESS.ADDR_0X76)
trykk = 0
def BMP280_sensor():
    global trykk
    trykk = BME280.pressure(BME280_P.PA)
```

<!-- Del 6.2: -->

## Oppgave 6: Skrive lufttrykk på OLED-skjerm

Vi skal gjøre det samme som vi gjorde for å vise den analoge verdien på OLED-skjermen:

    kitronik_VIEW128x64.show("Trykk: " + str(trykk) + " Pa", 4)


```python
def on_forever():
    BMP280_sensor()
basic.forever(on_forever)

BME280.power_on()
BME280.address(BME280_I2C_ADDRESS.ADDR_0X76)
trykk = 0
def BMP280_sensor():
    global trykk
    trykk = BME280.pressure(BME280_P.PA)
    kitronik_VIEW128x64.show("Trykk: " + str(trykk) + " Pa", 4)
```

<!-- Del 7: -->

## Oppgave 7: Formel for å regne om barometrisk lufttrykk til relativ høyde på CanSat @unplugged

![Regne-ut-hoyde-i-forhold-til-trykk.png](https://i.postimg.cc/Lst1zpZS/Regne-ut-hoyde-i-forhold-til-trykk.png)

Hvor:
- **h:**  Beregnet høyde i meter
- **h1:** Referansehøyde i meter (starthøyde er 0 eller m.o.h.)
- **T:**  Temperatur i Kelvin (``||variables: temperatur||`` + 273,15)
- **T1:** Referansetemperatur ved referansehøyden h1
- **R:**  Den spesifikke gasskonstant 287,06 J/kg K
- **a:**  Temperaturgradient, foreslått verdi -0,0065 K/m
- **p:**  Målt trykk i Pa
- **p1:** Trykk i Pa ved referansehøyden h1
- **g0:** Tyngdeakselerasjonen 9,81 m/s^2

<!-- Del 7.1: -->

## Oppgave 7: Lage formelen for å beregne høyden til CanSat

Vi skal lage en ny funksjon: ``||functions: høyde||``. Plasser den eneste blokken fra biblioteket ``||barometric-height: Høydeberegning||``.

Plasser ``||variabel: trykk||`` inn for P i formelen. Sjekk at alle verdiene er riktige!

```python
h = 0
trykk = 0
def høyde():
    global trykk
    global h
    h = høydeberegning.barometric_height(101325, 101325, 288, 0.0065, 0, 287, 9.81)
```


<!-- Del 7.2: -->

## Oppgave 7: Vise høyden til CanSat på OLED-skjerm

Vi skal igjen gjøre det samme som vi gjorde for å vise den analoge verdien på OLED-skjermen:

    kitronik_VIEW128x64.show("Høyde: " + str(h) + " m", 5)


```blocks
h = 0
trykk = 0
def høyde():
    global trykk
    global h
    h = høydeberegning.barometric_height(101325, 101325, 288, 0.0065, 0, 287, 9.81)
    kitronik_VIEW128x64.show("Høyde: " + str(h) + " m", 5)
}
```


<!-- Del 8: -->

## Sende og lagre data mellom 2 micro:bit





<!-- Del 9: -->

## Ferdig! 

Gratulerer! Du har nå løst alle oppgavene du trenger for å kunne programmere en fullstendig versjon av CanSat med bruk av micro:bit!

```python
def on_pin_pressed_p0():
    pass
input.on_pin_pressed(TouchPin.P0, on_pin_pressed_p0)

def on_received_number(receivedNumber):
    pass
radio.on_received_number(on_received_number)

def on_pin_released_p0():
    pass
input.on_pin_released(TouchPin.P0, on_pin_released_p0)

def on_logo_pressed():
    pass
input.on_logo_event(TouchButtonEvent.PRESSED, on_logo_pressed)

def on_button_pressed_a():
    global tekst
    if input.button_is_pressed(Button.A):
        input.set_accelerometer_range(AcceleratorRange.ONE_G)
        input.set_sound_threshold(SoundThreshold.LOUD, 128)
    elif input.acceleration(Dimension.X) == 0:
        # skjerm blokker
        led.plot(led.brightness(), 0)
        led.toggle(led.point_brightness(0, 0), 0)
        led.unplot(0, 0)
        led.plot_bar_graph(0, 0)
        led.plot_brightness(0, 0, 255)
        led.set_brightness(255)
        led.enable(False)
        led.stop_animation()
        led.set_display_mode(DisplayMode.BLACK_AND_WHITE)
    elif input.light_level() < 0:
        # radio blokker
        radio.set_group(1)
        radio.send_number(0)
        radio.send_value("name", 0)
        radio.send_string("")
        radio.set_transmit_power(7)
        radio.set_transmit_serial_number(True)
        radio.set_frequency_band(0)
        radio.raise_event(EventBusSource.MICROBIT_ID_BUTTON_A,
            EventBusValue.MICROBIT_EVT_ANY)
    elif input.pin_is_pressed(TouchPin.P0):
        pass
    elif input.is_gesture(Gesture.SHAKE):
        pass
    elif input.compass_heading() == 0:
        pass
    elif input.temperature() == 0:
        pass
    elif input.logo_is_pressed():
        pass
    elif "" == "":
        pass
    elif input.sound_level() < input.magnetic_force(Dimension.X) and input.rotation(Rotation.PITCH) == input.running_time():
        pass
    elif not (True) or False:
        pass
    elif input.running_time_micros() == 0 or 0 == 0:
        music.play(music.string_playable("- - - - - - - - ", 120),
            music.PlaybackMode.UNTIL_DONE)
        music.play(music.tone_playable(262, music.beat(BeatFraction.WHOLE)),
            music.PlaybackMode.UNTIL_DONE)
        music.ring_tone(262)
        music.rest(music.beat(BeatFraction.WHOLE))
        music.set_volume(music.volume())
        music.stop_all_sounds()
        music.change_tempo_by(music.beat(BeatFraction.WHOLE))
        music.set_tempo(music.tempo())
        music._play_default_background(music.built_in_playable_melody(Melodies.DADADADUM),
            music.PlaybackMode.IN_BACKGROUND)
        music.stop_melody(MelodyStopOptions.ALL)
        music.play(music.builtin_playable_sound_effect(soundExpression.giggle),
            music.PlaybackMode.UNTIL_DONE)
        music.play(music.create_sound_expression(WaveShape.SINE,
                5000,
                0,
                255,
                0,
                500,
                SoundExpressionEffect.NONE,
                InterpolationCurve.LINEAR),
            music.PlaybackMode.UNTIL_DONE)
        music.set_built_in_speaker_enabled(False)
    elif music.is_sound_playing():
        tekst = len(("" + str("this".split("")) + str(String.from_char_code(0).char_code_at(0))))
        tekst = parse_float("123")
        tekst = convert_to_text(0).char_at(0).substr(0, "this".compare("")).index_of("")
    elif led.point(0, 0):
        pass
    elif BMP280.temperature() == BMP280.pressure():
        BMP280.power_on()
        BMP280.power_off()
        BMP280.address(BMP280_I2C_ADDRESS.ADDR_0X76)
    elif Math.random_boolean():
        basic.show_number(0 * 0 - Math.PI + 0 / (min(max(Math.sqrt(Math.round(randint(0, 10))),
                abs(Math.constrain(0, 0, 0))),
            Math.map(0, 0, 1023, 0, 4)) % 1))
    elif "this".includes(""):
        pass
    elif "this".is_empty():
        pass
    elif pins.digital_read_pin(pins.map(0, 0, 1023, 0, 4)) == pins.analog_read_pin(AnalogPin.P0):
        pins.digital_write_pin(DigitalPin.P0, 0)
        pins.analog_write_pin(AnalogPin.P0, 1023)
        pins.analog_set_period(AnalogPin.P0, 20000)
        pins.set_audio_pin(DigitalPin.P0)
        pins.set_audio_pin_enabled(False)
        pins.servo_write_pin(AnalogPin.P0, 180)
        pins.servo_set_pulse(AnalogPin.P0, 1500)
    elif control.millis() == 0:
        control.wait_for_event(control.event_value(), 0)
        control.reset()
        control.wait_micros(control.event_timestamp())
        control.raise_event(EventBusSource.MICROBIT_ID_BUTTON_A,
            EventBusValue.MICROBIT_EVT_ANY)
    else:
        serial.write_line(serial.read_line())
        serial.write_number(0)
        serial.write_value("" + serial.read_until(serial.delimiters(Delimiters.NEW_LINE)) + serial.read_string(),
            0)
        serial.write_string("" + str((serial.read_buffer(0))))
        serial.write_numbers([0, 1])
        serial.redirect(SerialPin.P0, SerialPin.P1, BaudRate.BAUD_RATE115200)
        serial.redirect_to_usb()
        serial.set_tx_buffer_size(32)
        serial.set_rx_buffer_size(32)
        serial.set_write_line_padding(0)
        serial.set_baud_rate(BaudRate.BAUD_RATE115200)
input.on_button_pressed(Button.A, on_button_pressed_a)

def on_gesture_shake():
    BMP280.temperature()
    BMP280.pressure()
    BMP280.power_on()
    BMP280.power_off()
    BMP280.address(BMP280_I2C_ADDRESS.ADDR_0X76)
input.on_gesture(Gesture.SHAKE, on_gesture_shake)

def doSomething():
    pass

def on_received_string(receivedString):
    pass
radio.on_received_string(on_received_string)

def on_sound_loud():
    pass
input.on_sound(DetectedSound.LOUD, on_sound_loud)

def on_received_value(name, value):
    pass
radio.on_received_value(on_received_value)

def on_microbit_id_button_a_evt():
    pass
control.on_event(EventBusSource.MICROBIT_ID_BUTTON_A,
    EventBusValue.MICROBIT_EVT_ANY,
    on_microbit_id_button_a_evt)

def on_data_received():
    pass
serial.on_data_received(serial.delimiters(Delimiters.NEW_LINE), on_data_received)

def on_pulsed_p0_high():
    pass
pins.on_pulsed(DigitalPin.P0, PulseValue.HIGH, on_pulsed_p0_high)

def on_melody_note_played():
    pass
music.on_event(MusicEvent.MELODY_NOTE_PLAYED, on_melody_note_played)

teksttabell: List[str] = []
tabell: List[number] = []
tekst = 0
# basis blokker
basic.show_number(0)
basic.show_leds("""
    . . . . .
    . . . . .
    . . # . .
    . . . . .
    . . . . .
    """)
basic.show_icon(IconNames.HEART)
basic.show_string("Hello!")
basic.clear_screen()
basic.pause(100)
basic.show_arrow(ArrowNames.NORTH)
radio.set_group(1)
radio.send_number(0)
radio.send_value("name", 0)
radio.send_string("")
radio.set_transmit_power(7)
radio.set_transmit_serial_number(True)
radio.set_frequency_band(0)
radio.raise_event(EventBusSource.MICROBIT_ID_BUTTON_A,
    EventBusValue.MICROBIT_EVT_ANY)

def on_every_interval():
    pass
loops.every_interval(500, on_every_interval)

def on_forever():
    global tabell, teksttabell
    list2: List[number] = []
    for index in range(4):
        pass
    # løkker blokker
    while False:
        pass
    for verdi in tabell:
        pass
    for indeks in range(5):
        pass
    for index2 in range(5):
        pass
    for index3 in range(radio.received_packet(RadioPacketProperty.SIGNAL_STRENGTH)):
        continue
        break
    # Logikk blokker
    if True:
        pass
    if True:
        pass
    else:
        pass
    kitronik_VIEW128x64.control_display_on_off(kitronik_VIEW128x64.on_off(False))
    kitronik_VIEW128x64.set_font_size(kitronik_VIEW128x64.FontSelection.NORMAL)
    kitronik_VIEW128x64.refresh()
    kitronik_VIEW128x64.invert(kitronik_VIEW128x64.on_off(False))
    kitronik_VIEW128x64.show(0)
    kitronik_VIEW128x64.set_pixel(0, 0)
    kitronik_VIEW128x64.plot(0)
    kitronik_VIEW128x64.draw_line(kitronik_VIEW128x64.LineDirectionSelection.HORIZONTAL,
        10,
        0,
        0)
    kitronik_VIEW128x64.draw_rect(60, 30, 0, 0)
    kitronik_VIEW128x64.clear_line(1)
    kitronik_VIEW128x64.clear_pixel(0, 0)
    kitronik_VIEW128x64.clear()
    # funksjoner blokker
    doSomething()
    # tabeller blokker
    tabell = [len(tabell), 2, 3]
    teksttabell = ["ei / en / ett", "b", "c"]
    tabell[list2.remove_at(list2._pick_random())] = list2.shift()
    tabell.append(tabell[list2.pop()])
    tabell.pop()
    tabell = []
    tabell[0] = 0
    list2.append(list2.unshift(0))
    list2.pop()
    tabell.shift()
    list2.unshift(0)
    list2.insert_at(0, list2.index_of(0))
    list2.remove_at(0)
    list2.reverse()
basic.forever(on_forever)

def on_forever2():
    pass
basic.forever(on_forever2)

def on_in_background():
    pass
control.in_background(on_in_background)
BME280.humidity()
BME280.temperature(BME280_T.T_C)
BME280.pressure(BME280_P.PA)
BME280.dewpoint()
def onTemperature_higher_than():
    pass
BME280.temperature_higher_than(30, onTemperature_higher_than)
def onTemperature_below_than():
    pass
BME280.temperature_below_than(10, onTemperature_below_than)
def onHumidity_higher_than():
    pass
BME280.humidity_higher_than(50, onHumidity_higher_than)
def onHumidity_below_than():
    pass
BME280.humidity_below_than(10, onHumidity_below_than)
def onPressure_higher_than():
    pass
BME280.pressure_higher_than(100000, onPressure_higher_than)
def onPressure_below_than():
    pass
BME280.pressure_below_than(100000, onPressure_below_than)
BME280.power_on()
BME280.power_off()
BME280.address(BME280_I2C_ADDRESS.ADDR_0X76)
```

```package
pxt-kitronik-128x64display=github:kitronikltd/pxt-kitronik-128x64display
BMP280=github:microbit-makecode-packages/BME280
barometric-height=github:oysa88/barometric-height
```
