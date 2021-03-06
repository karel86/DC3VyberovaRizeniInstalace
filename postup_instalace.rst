Postup instalace
===============================

Následující kapitola popisuje postup nasazení veřejné části modulu **Výběrová řízení** na IIS.

Instalace webové aplikace
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Na webovém serveru **veřejné části** v IIS je třeba vytvořit nový aplikační pool s názvem např. **DC3VyberovaRizeniPool**. Pool musí být nastaven jako **No managed code** (Bez spravovaného kódu).

.. image:: /Img/PoolAVR.PNG

2. V rozšiřujícím nastavení poolu je třeba zapnout vlastnost **Load User Profile** (Načíst profil uživatele) = True.

.. image:: /Img/PoolAVR2.PNG

3. Vytvořit novou webovou aplikaci a nasměrovat ji na adresář s buildem veřejné části modulu Výběrová řízení. Jako pool zvolit nově vytvořený.

.. image:: /Img/PoolAVR3.PNG

Import certifikátů
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Aby se veřejná část modulu **Výběrová řízení** mohla dotazovat na data do DC3, je nutné na **veřejný server** 
naimportovat příslušné certifikáty.

1. Spustit konzolu pro import certifikátů přes Start > Run > mmc

2. Přes menu Soubor -> Přidat nebo odebrat modul snap-in vybrat Certifikáty a zvolit certifikáty na tomto počítači.

.. image:: /Img/ConsoleCert.PNG

3. Spustit import klientského certifikátu pro ověřování. Spuštění proveďte pomocí .cmd souboru **InstallCertVyberovaRizeni.cmd** spustit jako správce).

4. Import provede nahrání certifikátu **DC3 CA** a vlastního klientského certifikátu **DC3VyberovaRizeni**. Certifikát DC3 CA musí být přítomen ve složce **Trusted root authorities** (Důvěryhodné kořenové certifikační autority) a vlastní klientský certifikát **DC3VyberovaRizeni** by se měl objevit ve složce **Personal** (Osobní). Správnost nahrání klientského certifikátu DC3VyberovaRizeni lze ověřit tak, že při jeho otevření by měl být **validní**. Pokud by se certifikát z nějakého důvodu nenaimportoval, je třeba ho naimportovat ručně (heslo uvedené v .cmd souboru).

.. image:: /Img/ConsoleCert3.PNG

5. Nyní je nutné pro naimportovaný certifikát nastavit oprávnění na Private key. Pomocí **pravého tlačítka -> All tasks -> Manage private keys...** přidat účet **IIS APPPOOL\\<jmeno poolu>**. Např. IIS APPPOOL\\DC3VyberovaRizeniPool. Úroveň oprávnění nastavit na **Full control**.

.. image:: /Img/CertPerm.PNG


Nastavení oprávnění pro DB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Veřejná část modulu Výběrová řízení provádí ukládání dat z registračních formulářů do lokální embedded
databáze (SQL LocalDB). Databázi tvoří 2 soubory **DC3VyberovaRizeni.mdf** a **DC3VyberovaRizeni.ldf**
umístěné v adresáři s ostatními soubory webové aplikace.

Aby bylo možné do DB přistupovat, je nutné na oba soubory nastavit stejná oprávnění pro identitu IIS
Poolu, jako v případě nastavení oprávnění na klientský certifikát.

.. image:: /Img/CertPerm2.PNG

Konfigurace webové aplikace
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

V tomto kroku je třeba nastavit konfigurační soubor appsettings.json pro správné nastavení zobrazování zveřejněných inzerátů.

1. Otevřít soubor **appsettings.json**

2. Nastavit klíč na **DC3BaseUrl**. Url musí směřovat na interní DC3 kde je zapnut modul Výběrová řízení

3. Zkontrolovat klíč **TypInzeratuKod**. Měl by obsahovat kód číselníkové položky z číselníku **Typ inzerátu**. Všechny inzeráty zveřejněné pod tímto typem budou vypsány v seznamu volných pozic ve veřejné části modulu Výběrová řízení. Default je Externi.

4. Nastavit klíč **Theme** pro zobrazení správného brandingu pro zvoleného zákazníka.

5. Nastavit další klíče uvedené v sekci Pages. Pro správné popisy stránek, patičky, SEO apod.

Zveřejnění inzerátu
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Po úspěšném dokončení předchozích kroků je třeba otestovat, zda vypsaná výběrová řízení jsou
zobrazována ve veřejné části na základě jejich inzerátů, platnosti a platnosti zveřejnění.

1. Přepnout se do **administrace** výběrových řízení a založit platné výběrové řízení.

.. image:: /Img/VyberoveRizeni.PNG

2. Pro výběrové řízení založit **inzerát** a zařadit ho do **typu** pro zveřejnění na webu (Externi)

.. image:: /Img/Inzerat.PNG

3. Nastavit platné **zveřejnění** inzerátu

.. image:: /Img/InzeratZverejneni.PNG

4. Pokud je vše nastaveno správně, uvedený inzerát se zobrazí ve veřejné části a je možné provést registraci uchazeče skrze registrační formulář.
