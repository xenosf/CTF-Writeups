# Substitution Extreme

**Category**: Crypto

728 points

----

## Challenge Description
You have intercepted the following transmission. What is the first six words of the decrypted transmission?

Flag is the answer to the question enclosed with the flag format: STC{WORD1 WORD2...}

**Author:** icebear

## Hint (100pts)
Nope, not in English

## Attached files/Instructions
* transmission.txt

### transmission.txt
```
EZWUFROD HOPVGWUEG GIGRGB HZFUGB PZVGEG WURGU AGPV MZERZMGD IO HZRGMGP HZJZPGPQUPV JZRGAU IO GHOG MZPVVGEG. HOPVGWUEG MZEWOHGB IGEO JGRGAHOG IZPVGP HZRGM QXBXE IO UMGEG, IGP DZWURGUGP EOGU OPIXPZHOG IZPVGP HZRGM HOPVGWUEG IO HZRGMGP. HOPVGWUEG GIGRGB HGRGB HGMU WUHGM DZKGPVGP MZEUPVVUR, IGP IODZPGRO HZFGVGO HZFUGB FGPIGEGAG VRXFGR JZMEXWXROMGP AGPV JZJGOPDGP WZEGPGP WZPMOPV IGRGJ WZEIGVGPVGP IGP DZKGPVGP GPMGEGFGPVHG. WZRGFUBGP HOPVGWUEG JZEUWGDGP HGRGB HZFUGB IGEOWGIG ROJG WZRGFUBGP MZEHOFUD IUPOG.

HUJFZE: KODOWZIOG
```

----
## Solution
Originally, I tried to decode it, using multiple tools and trying manually, to no avail. Then, I bought the hint and facepalmed at my own oversight.

I found a web tool, [quipqiup](https://www.quipqiup.com/) that could decode substitution ciphers in multiple languages. I put that in, and got the answer.

<!--screenshot-->

```
REPUBLIK SINGAPURA ADALAH SEBUAH NEGARA PULAU YANG TERLETAK DI SELATAN SEMENANJUNG MELAYU DI ASIA TENGGARA. SINGAPURA TERPISAH DARI MALAYSIA DENGAN SELAT JOHOR DI UTARA, DAN KEPULAUAN RIAU INDONESIA DENGAN SELAT SINGAPURA DI SELATAN. SINGAPURA ADALAH SALAH SATU PUSAT KEWANGAN TERUNGGUL, DAN DIKENALI SEBAGAI SEBUAH BANDARAYA GLOBAL METROPOLITAN YANG MEMAINKAN PERANAN PENTING DALAM PERDAGANGAN DAN KEWANGAN ANTARABANGSA. PELABUHAN SINGAPURA MERUPAKAN SALAH SEBUAH DARIPADA LIMA PELABUHAN TERSIBUK DUNIA. 

SUMBER: WIKIPEDIA
```

For fun, I went to check which page this was from. It was the Indonesian Wikipedia page on Singapore. Fun!

<!--screenshot-->

### Flag:
```
STC{REPUBLIK SINGAPURA ADALAH SEBUAH NEGARA PULAU}
```
