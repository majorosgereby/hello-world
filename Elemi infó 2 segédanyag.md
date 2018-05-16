# Elemi infó 2 segédanyag

## Reni által összeírt help PYTHONBAN

### importálás
import valami egész csomagot hozza.
	pl.: 
```py
import csomagnév
```
	VAGY: 
```py
import csomagnév as rövidítés
```
	
másik importálási módszer: konkrétan tudom, hogy a csomag melyik részét szeretném importálni
```py
from csomagnév import rész
```

importálható csomagok:
	mesteresítéshez: sys -> stdin : stream data in : 
						 -> stdout: stream data out
```py
from sys import stdin,stdout
```

### bejárás-iterálás-map
```py					
tömb=['a','asd','fghj','PQWET']
#bejárás
for i in range(len(tömb)):
    tömb[i]
#iterálás
for x in tömb:
    x
#feltérképezés
map(josh,tömb)

def josh(x):
    return len(x)
	
```
### Időkezelés
```py
from datetime import date,time

#17 óra
t1=time(17)
#17:20
t2=time(17,20)
#17:20:36
t3=time(17,20,36)
```

### másik trükk időre pythonban
```py
from datetime import time as t
from datetime import date as d
from datetime import datetime as dt

t1=t(10,20,30)
print(t1)
print(int(str(t1).split(':')[2]))

print(t1.second) #szebb, lágyabb, jobb. egyszerűbb. mint felette. :D

t2=t(5,40,11)

print(dt.combine(d.min,t1)-dt.combine(d.min,t2)) #még véletlenül sincs semmi köze a min-nek a minute-hez. :D

def időkivonás(később,korább):
    return dt.combine(d.min,később)-dt.combine(dt.min,korább)

print(időkivonás(t1,t2))
```


## Győző által írt Pythonos mester kompatibilis feltöltés
```py
from sys import stdin, stdout
def main():
    n = int(stdin.readline()) #n az elemek száma (sorok?)
    szamok = list(map(int, stdin.readline().split()))
    osszeg = sum(szamok)
    szorzat = 1
    for x in szamok:
        szorzat *= x
    stdout.write(str(osszeg) + '\n' + str(szorzat) + '\n')
main()
```

## 2018-as májusi emelt érettségi HIVATALOS C++ megoldása
```cpp
#include <iostream>
#include <fstream>
using namespace std;
struct Tmozgas{
	int id;
	string irany;
	int ido, ora, perc;
};
int main()
{
    Tmozgas mozgas[1000];
    // 1. feladat
    ifstream be("ajto.txt");
    int i=0;
    while(be >> mozgas[i].ora >> mozgas[i].perc >> mozgas[i].id >> mozgas[i].irany)
	{
		mozgas[i].ido=mozgas[i].ora*60+mozgas[i].perc;
		i++;
	}
	int db=i;
	be.close();

    cout << endl << "2. feladat" << endl;
    int utolso=0;
    for(int i=0; i<db; i++)
	{
		if(mozgas[i].irany=="ki")
		{
			utolso=i;
		}
	}
	cout << "Az elso belepo: " << mozgas[0].id << endl;
	cout << "Az utolso kilepo: " << mozgas[utolso].id << endl;

	// 3. feladat
	int athaladt[101]={0};
	for(int i=0; i<db; i++)
	{
		athaladt[mozgas[i].id]++;
	}
	ofstream ki("athaladas.txt");
	for(int i=1; i<=100; i++)
	{
		if(athaladt[i]>0)
		{
			ki << i << " " << athaladt[i] << endl;
		}
	}
	ki.close();

	cout << endl << "4. feladat" << endl;
	cout << "A vegen a tarsalgoban voltak:";
	for(int i=1; i<=100; i++)
	{
		if(athaladt[i]%2==1) cout << " " << i;
	}
	cout << endl;

    cout << endl << "5. feladat" << endl;
    int mikor=0, letszam=0, maxletszam=0;
    for(int i=0; i<db; i++)
	{
		if(mozgas[i].irany=="be") letszam++;
		else letszam--;
		if(letszam>maxletszam)
		{
			maxletszam=letszam;
			mikor=i;
		}
	}
	cout << "Peldaul " << mozgas[mikor].ora << ":" << mozgas[mikor].perc << "-kor voltak a legtobben a tarsalgoban." << endl;

    cout << endl << "6. feladat" << endl;
    int sz;
    cout << "Adja meg a szemely azonositojat! ";
    cin >> sz;

    cout << endl << "7. feladat" << endl;
    int hanyszor=0;
    for(int i=0; i<db; i++)
	{
		if(mozgas[i].id==sz)
		{
			if(mozgas[i].irany=="be") cout << mozgas[i].ora << ":" << mozgas[i].perc << "-";
			else cout << mozgas[i].ora << ":" << mozgas[i].perc << endl;
		}
	}
	cout << endl;

    cout << endl << "8. feladat" << endl;
    int hossz=0;
    string merre;
    for(int i=0; i<db; i++)
	{
		if(mozgas[i].id==sz)
		{
			merre=mozgas[i].irany;
			if(merre=="be") hossz-=mozgas[i].ido;
			else hossz+=mozgas[i].ido;
		}
	}
	if(merre=="be")
	{
		hossz+=15*60;
		cout << "A(z) " << sz << ". szemely osszesen " << hossz << " percet volt bent, a megfigyeles vegen a tarsalgoban volt." << endl;
	}
	else
	{
		cout << "A(z) " << sz << ". szemely osszesen " << hossz << " percet volt bent, a megfigyeles vegen nem volt a tarsalgoban." << endl;
	}

    return 0;
}
```

## 2018-as májusi emelt érettségi BÓBITA FÉLE pas megoldása
```pas
program tarsalgo;

type TMozgas= Record
			ora: integer;
			perc: integer;
			id: integer;
			irany: string;
			ido: integer;
		end;
		
	Ttomb=Array[1..1000] of TMozgas;
	
var f:text;
	N, utolso, i, mikor, letszam, maxletszam, azon, hossz: integer;
	mozgas: Ttomb;
	athaladt: Array[1..100] of integer;
	merre: string;

BEGIN
//BEOLVASÁS
	assign(f,'ajto.txt');
	{$I-}
	reset(f);
	{$I+}
	if IOResult<>0 then begin
                    writeln('Hiba: nincs meg a file.');
                    halt;
                    end;
    N:=1;
    while not eof(f) do begin
		Readln(f, mozgas[N].ora, mozgas[N].perc, mozgas[N].id, mozgas[N].irany);
		mozgas[N].ido:=mozgas[N].ora*60+mozgas[N].perc;
		inc(N);
    end;
    N:=N-1;
    close(f);
    
//BEOLVASÁS VÉGE
	
//2. feladat
	utolso:=1;
	for i:=1 to N do
	Begin
		if (mozgas[i].irany=' ki') then utolso:=i;
	End;
	Writeln('2. feladat');
	Writeln('Az elso belepo: ',mozgas[1].id);
	Writeln('Az utolso belepo: ',mozgas[utolso].id);
	Writeln();
	
//3. feladat
	for i:=1 to 100 do
	Begin
		athaladt[i]:=0;
	End;
	for i:=1 to N do
	Begin
		inc(athaladt[mozgas[i].id]);
	End;
	assign(f,'athaladas.txt');
	rewrite(f);
	for i:=1 to length(athaladt) do
	Begin
		if (athaladt[i]<>0) then Writeln(f, i, ' ', athaladt[i]);
	End;
	close(f);
	
//4. feladat
	Writeln('4. feladat');
	Write('A vegen a tarsalgoban voltak: ');
	for i:=1 to length(athaladt) do
	Begin
		if (athaladt[i] mod 2 = 1) then
		Begin
			Write(i, ' ');
		End;
	End;
	Writeln();
	Writeln();
	
//5. feladat
	Writeln('5. feladat');
	mikor:=0;
	letszam:=0;
	maxletszam:=0;
	for i:=1 to N do
	Begin
		if (mozgas[i].irany=' be') then
			Begin
				inc(letszam);
			End
		else 
			Begin
				letszam:=letszam-1;
			End;
		if (letszam>maxletszam) then
		Begin
			maxletszam:=letszam;
			mikor:=i;
		End;
	End;
	Writeln('Peldaul ', mozgas[mikor].ora, ':', mozgas[mikor].perc, '-kor voltak a legtobben a tarsalgoban');
	Writeln();
	
//6. feladat
	Writeln('6.feladat');
	Write('Adja meg a szemely azonositojat! ');
	Readln(azon);
	Writeln();

//7. feladat
	Writeln('7. feladat');
	for i:=1 to N do
	Begin
		if (mozgas[i].id=azon) then
		Begin
			if (mozgas[i].irany=' be') then Write(mozgas[i].ora,':',mozgas[i].perc,'-')
			else Writeln(mozgas[i].ora,':',mozgas[i].perc);
		End;
	End;
	Writeln();
	
//8. feladat
	Writeln('8. feladat');
	hossz:=0;
	for i:=1 to N do
	Begin
		if(mozgas[i].id=azon) then
		Begin
			merre:=mozgas[i].irany;
			if(merre=' be') then hossz:= hossz-mozgas[i].ido
			else hossz:= hossz+mozgas[i].ido;
		End;
	End;
	if (merre=' be') then
	Begin
		hossz:= hossz+15*60;
		Writeln('A(z) ', azon, '. szemely osszesen ',hossz,' percet volt bent, a megfigyeles vegen a tarsalgoban volt.'); 
	End
	else
	Begin
		Writeln('A(z) ', azon, '. szemely osszesen ',hossz,' percet volt bent, a megfigyeles vegen nem volt a tarsalgoban.'); 
	End;
END.
```

## Reni által megoldott beadandó feladat PYTHONBAN
```py
befájlneve=input("Kérem a fájl nevét!")
befájl=open(befájlneve,'r')
elemekszáma=int(befájl.readline())
termékek=[]
for i in range(elemekszáma):
    termékek.append(befájl.readline().split('\n')[0])
ARU=befájl.readline().split('\n')[0]
darabszám=int(befájl.readline())
sorszám=int(befájl.readline())
befájl.close()
def tombkiir(tömb):
    for i in tömb:
        print(i)
#tombkiir(termékek)

def elso(termékektömb):
	print('#')
	db=0
	for i in termékektömb:
	    if i=='F':
        	db+=1
	print(db)

def masodik(tömb):
	print('#')
	db=0
	i=0
	while tömb[i]!='F':
		db+=1
		i+=1
	print(db)

def vdarabszámok(ARU,tömb):
	fdb=0
	vásárlósorszámok=[]
	for i in tömb:
		if ARU==i:
			if (fdb+1) not in vásárlósorszámok:
				vásárlósorszámok.append(fdb+1)
		if i=='F':
			fdb+=1
	return vásárlósorszámok

def harmadik(ARU, tömb):
	print('#')
	valami=vdarabszámok(ARU,tömb)
	print(valami[0])
	print(valami[len(valami)-1])

def negyedik(ARU, tömb):
	print('#')
	valami=vdarabszámok(ARU,tömb)
	print(len(valami))

def fiz(dbszám):
	fizetendő=0
	if (dbszám==1):
		fizetendő=500
	if (dbszám==2):
		fizetendő=950
	if (dbszám>2):
		fizetendő=950+(dbszám-2)*400
	return fizetendő
def otodik(dbszám):
	print('#')
	print(fiz(dbszám))

def adottsorszámúvásárlótermékei(sorsz,tömb):
	fdb=0
	i=0
	vásárlásai=[]
	egyelembőlhánydarabotvett=[]
	while sorsz!=fdb:
		if sorsz==fdb+1:
			try:
				#vásárlásai.index(tömb[i])
				if (tömb[i]!='F'):
					egyelembőlhánydarabotvett[vásárlásai.index(tömb[i])]+=1
			except:
				vásárlásai.append(tömb[i])
				egyelembőlhánydarabotvett.append(1)
		if tömb[i]=='F':
			fdb+=1
		i+=1
	return (vásárlásai,egyelembőlhánydarabotvett)
def hatodik(sorsz, tömb):
	print('#')
	a,b=adottsorszámúvásárlótermékei(sorsz,tömb)
	print(len(a))
	for i in range(len(a)):
		print(a[i], b[i])

def hetedik(tömb):
	print('#')
	fdb=0
	s=''
	for i in tömb:
		if i=='F':
			fdb+=1
	for i in range(fdb):
		a,b=adottsorszámúvásárlótermékei(i+1,tömb)
		osszeg=0
		for j in b:
			osszeg+=fiz(j)
		s+=str(osszeg)
		s+=' '
	print(s)

elso(termékek)
masodik(termékek)
harmadik(ARU, termékek)
negyedik(ARU, termékek)
otodik(darabszám)
hatodik(sorszám,termékek)
hetedik(termékek)
```