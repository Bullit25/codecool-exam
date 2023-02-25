# codecool-exam
documentation of my final exam

### sell-1 mappa feladatai:

### 1. Feladat:
Tömörítsd ki a task1-xdg.tar.gz archív állományt és módosítsd a kitömörített fájlstruktúrát

Az archív fájl tartalma a következő:
```
fmt
└── [drwxr-xr-x] xdg
░░░░├── [drwxr-xr-x] automatization
░░░░│░░░├── [-rw-r--r--] auto-cleaner.desktop
░░░░│░░░├── [-rw-r--r--] cleaner-configurator.desktop
░░░░│░░░└── [-rw-r--r--] xdg-user-dirs.desktop
░░░░├── [drwxr-xr-x] syscfg
░░░░│░░░└── [lrwxrwxrwx] default.conf -&gt; ../account-dirs.defaults
░░░░├── [-rw-r--r--] account-dirs.conf
░░░░└── [-rw-r--r--] account-dirs.defaults
```
- Tömörítsd ki az állományt (a munka könyvtáradban ezután létezzen az fmt mappa)
- Töröld le az syscfg mappát és benne minden fájlt (egyéb fájlokat ne törölj)
- Az account-dirs.conf ne legyen olvasható akárki számára (csak a tulajdonos és a
csoport tudja olvasni)
- Az account-dirs.conf fájl végéhez fűzd hozzá az new_account=AccountName szöveget
- Az account-dirs.conf fájl eredeti tartalma ne törlődjön/módosuljon (csak kerüljön a
végére az előbbi szöveg)

#### task1.sh tartalma:
```bash
#!/bin/bash
tar -xzf task1-xdg.tar.gz
rm -fr fmt/xdg/syscfg
chmod 640 /fmt/xdg/account-dirs.conf
echo 'new_account=AccountName' >> fmt/xdg/account-dirs.conf
```
### 2. Feladat:
Keresd meg a megfelelő adatokat a task2-oscars.csv fájlban és mentsd el őket (task2.sh)

A fájlban Oscar-díjas férfi színészek neve, életkora és a film címe és megjelenési éve található
amiért díjat kaptak 4 oszlopba rendezve vesszővel elválasztva:
Megjelenés éve,Színész életkora,Színész neve,Film címe
1995,38,Tom Hanks,Forrest Gump

- Mentsd le a fájl első 8 sorát a first8.txt-be
- Mentsd le a Ric szót tartalmazó sorokat a ric.txt-be
- Mentsd le a '90-as években megjelent filmek címeit a movies.txt-be
- Mentsd le a 29 éves színészek nevét az age.txt-be
- Mentsd le a '80-es években megjelent filmekben szereplő 30-es éveiben járó színészek nevét
az actors.txt-be

#### task2.sh tartalma:
```bash
#!/bin/bash
F='task-oscars.csv'
head -n 8 $F > firstu8.txt
cat $F | grep 'Ric' > ric.txt
cat $F | grep '^199' | awk -F ',' '{print $4}' > movies.txt
cat $F | awk -F ',' '{if($2="29"){print $3}}' > age.txt
cat $F | grep '^198' | awk -F ',' '{print $2,$3}' | grep '^3' | awk -F ' ' '{print $2,$3}' > actors.txt
```

## shell-2 mappa feladatai:

### 1. Feladat:
Készítsd el a következő könyvtárszerkezetet a megfelelő fájlokkal és jogosultságokkal
(task1.sh)
```
configuration
├── [drwxr-x---] api-connections
│░░░├── [-rw-------] api-key.txt
│░░░└── [-rw-rw-r--] api-connections.txt
├── [drwxr-xr-x] links
│░░░└── [lrwxrwxrwx] default.conf -> ../settings.defaults
└── [-rw-r--r--] settings.defaults
```
#### task-1.sh tartalma:
```bash
#!/bin/bash
mkdir -p configuration/api-connections
mkdir -p configuration/links
touch configuration/api-connections/api-key.txt.txt
touch configuration/api-connections/api-connections.txt
touch configuration/settings.defaults
ln -s ../settings.defaults configuration/links/default.conf
chmod 750 configuration/api-connections
chmod 755 configuration/links
chmod 600 configuration/api-connections/api-key.txt.txt
chmod 664 configuration/api-connections/api-connections.txt
chmod 777 configuration/links/default.conf
chmod 644 configuration/settings.defaults
```
### 2. Feladat:
Töltsd le az archív fájlt, csomagold ki és adj hozzáférést hozzá másoknak a fájlokhoz(task2.sh)

Az archív fájl tartalma a következő:
netserver
└── html
░░░░└── index.html
└── static
░░░░└── style.css
- Töltsd le a netserver.tar.gz archívumot a https://github.com/CodecoolBase/sysadmin-exam-assets/raw/master/netserver.tar.gz linkről
- Az archívum netserver.tar.gz néven kerüljön mentésre/letöltésre
- Csomagold ki az archívumot és helyezd át a kicsomagolt mappastruktúrát a /tmp
mappába, az alá
- Állítsd be a /tmp/netserver könyvtár felhasználói csoportját, hogy az exam csoport
tulajdona legyen
- Állítsd be ugyanezt minden fájlra a /tmp/netserver mappán belül

#### task2.sh tartalma:
```bash
#!/bin/bash
wget https://github.com/CodecoolBase/sysadmin-exam-assets/raw/master/netserver.tar.gz
tar xzf netserver.tar.gz -C /tmp/
chown -R :exam /tmp/netserver
```
## sql-1 mappa feladatai:

A képen látható northwind DB séma a Unix felhasználód nevével azonos nevű DB-be van
betöltve (tehát, ha john.doe@example.com az email címed, akkor a DB neve john.doe)

### 1. Feladat:
Figyelem: minden lekérdezést külön fájlba kell írnod (task1a.sql, task2b.sql, stb.)
1 Kérdezd le a megfelelő adatokat a "products" táblából
- Jelenítsd meg az összes terméket, azok minden adatát (task1a.sql)
- Jelenítsd meg a termékek nevét amikből több mint 20 db van raktáron (units_in_stock)
(task1b.sql)
- Jelenítsd meg a termékek nevét amikből több mint 20 db van raktáron és amelyek egységára
(unit_price) nagyobb mint 10 (task1c.sql)
- Jelenítsd meg azon termékek azonosítóját, nevét és egység mennyiségét
(quantity_per_unit) amik egység mennyisége tartalmazza az "oz" (uncia) karaktersort és nem
0 van belőle raktáron (units_on_order), rendezd a lekérdézés eredményét növekvő sorrendbe
a terméknév alapján (task1d.sql)

#### task1a.sql tartalma:
```sql
SELECT * FROM products;
```
#### task1b.sql tartalma:
```sql
SELECT product_name FROM products WHERE units_in_stocsk>20;
```
#### task1c.sql tartalma:
```sql
SELECT product_name FROM products WHERE units_in_stocsk>20 AND unit_price >10;
```
#### task1d.sql tartalma:
```sql
SELECT product_id, product_name, quantity_per_unit 
FROM products WHERE quantity_per_unit LIKE '%oz%' AND units_on_order > 0
ORDER BY product_name ASC;
```

### 2. FEALADAT:
Kérdezd le a megfelelő adatokat a "products" és "order_details" táblákból
- Jelenítsd meg a termékek nevét és az adott termékre leadott rendelések összértékét
(unit_price és quantity alapján), az oszlopok neve legyen "product" és "stock_price"
(task2a.sql)

#### task2a.sql tartalma:
```sql
SELECT p.product_name AS product, SUM(od.unit_price * od.quantity) AS stock_price
FROM products AS p 
INNER JOIN order_details AS od ON p.product_id = od.product_id
GROUP BY p.product_name;
```

### 3. FEALADAT:
Kérdezd le a megfelelő adatokat a "products" és "categories" táblákból
- Jelenítsd meg a "Seafood" kategóriába tartozó termékek nevét (task3a.sql)
- Jelenítsd meg azon termék kategóriák nevét és az adott kategóriába tartozó termékek számát
amelyekbe több mint 10 termék tartozik (task3b.sql)
sql-2

#### task3a.sql tartalma:
```sql
SELECT products.product_name AS product_name FROM products
JOIN categories
ON products.category_id=categories.category_id
WHERE category_name='Seafood';
```

#### task3b.sql tartalma:
```sql
SELECT categories.category_name, COUNT(*)
FROM products
JOIN categories
ON products.category_id=categories.category_id
GROUP BY categories.category_name
HAVING COUNT(*) > 10;
```

## sql-2 mappa feladatai:

### 1. FELADAT:
Módosítsd az adatbázis "products" táblájának szerkezetét és tartalmát (task1.sql)
- Adj hozzá a "products" táblához egy új, max. 255 karakter hosszú szöveges adatokat tárolni
képes "description" (leírás) nevű oszlopot
- A "description" oszlop alapértelmezett értéke legyen 'N/A'
- Hozz létre új rekordot a "products" táblában, legyen az elsődleges kulcs értéke 80, a neve
'Ütvefúró', a 'discontinued' értéke 0, a leírás (description) pedig 'Satisfaction'
- Frissítsd a "products" táblában a 19-es azonosítóval rendelkező rekordot, aminek a neve
'Teatime Chocolate Biscuits' és nevezd át 'Coffeetime Chocolate Biscuits'-re

#### task1.sql tartalma:
```sql
ALTER TABLE public.products
    ADD COLUMN description varchar(255) DEFAULT 'N/A';
INSERT INTO description
VALUES (80, 'Ütvefúró', 0, 'Satisfaction');
UPDATE products SET name='Coffeetime Chocolate Biscuits' WHERE product_id=19;
```
### 2. FELADAT:
Végezd el az adminisztratív feladatokat az adatbázis szerveren (task2.sql)
- Hozz létre egy "student" nevű táblát az adatbázisban, három oszloppal: "id", "name" és
"active"
- Az "id" oszlop egészszám típusú, és elsődleges kulcs
- A "name" karakter típusú, max. 128 karakter hosszú szöveg, és nem lehet üres
- Az "active" oszlop boolean típusú és a alapértéke "true"

#### task2.sql tartalma:
```sql
CREATE TABLE public.student
(
    id INTEGER PRIMARY KEY,
    name varchar(128) NOT NULL,
    active BOOLEAN DEFAULT true
);
```

## clod-1 mappa feladatai:
Hozz létre egy security group-ot:
- frankfurti régióban hozd létre (eu-central-1)
- a régió default VPC-jében
- a neve legyen az email címed eleje "-Gov2a" toldalékkal, pl. "joe.smith@codecool.com"
esetében "joe.smith-Gov2a"
- a "Name" tag értéke legyen ua., mint a security group neve (figyelem, nem ua. a kettő)
- engedélyezd a bejövő SSH kapcsolatokat a 22-es TCP porton bármilyen forrásból

### 2. FELADAT:
Indíts egy EC2 instance-t:
- frankfurti régióban hozd létre (eu-central-1)
- a régió default VPC-jében
- használd az Ubuntu Server 18.04 LTS AMI-t
- az instance type legyen t2.micro
- adj hozzá "Name" tag-et/címkét
- a "Name" tag értéke legyen az email címed eleje "-Gov2a" toldalékkal, pl.
"joe.smith@codecool.com" esetében "joe.smith-Gov2a"
- a korábban létrehozott "-Gov2a" végződésű névvel ellátott security group-ot rendeld hozzá az
instance-hoz

6
### 3. FELADAT:
Hozz létre egy másik security group-ot:
- frankfurti régióban hozd létre (eu-central-1)
- a régió default VPC-jében
- a neve legyen az email címed eleje "-Gov2b" toldalékkal, pl. "joe.smith@codecool.com"
esetében "joe.smith-Gov2b"
- a "Name" tag értéke legyen ua., mint a security group neve (figyelem, nem ua. a kettő)
- engedélyezd a bejövő SSH kapcsolatokat a 22-es TCP porton az előbbi lépésben indított EC2
instance privát IP címéről

### 4. FELADAT:
Indíts egy másik EC2 instance-t:
- frankfurti régióban hozd létre (eu-central-1)
- a régió default VPC-jében
- használd az Ubuntu Server 18.04 LTS AMI-t
- az instance type legyen t2.micro
- adj hozzá "Name" tag-et/címkét
- a "Name" tag értéke legyen az email címed eleje "-Gov2b" toldalékkal, pl.
"joe.smith@codecool.com" esetében "joe.smith-Gov2b"
- a korábban létrehozott "-Gov2b" végződésű névvel ellátott security group-ot rendeld hozzá az
instance-hoz