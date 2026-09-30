# kosik3-0

sklad = {
    "ovocie": {
        "jablko": {"cena": 0.80, "mnozstvo": 3},
        "banan": {"cena": 1.50, "mnozstvo": 2},
        "hruska": {"cena": 1.20, "mnozstvo": 5},
        "marhula": {"cena": 2.00, "mnozstvo": 4},
        "slivka": {"cena": 1.80, "mnozstvo": 6}
    },
    "zelenina": {
        "mrkva": {"cena": 0.60, "mnozstvo": 10},
        "petrzlen": {"cena": 0.90, "mnozstvo": 8},
        "celer": {"cena": 1.10, "mnozstvo": 3},
        "zemiak": {"cena": 0.70, "mnozstvo": 15}
    },
    "sladkosti": {
        "cokolada": {"cena": 1.50, "mnozstvo": 5},
        "cukor": {"cena": 1.20, "mnozstvo": 10}
    },
    "mliecne": {
        "mlieko": {"cena": 1.00, "mnozstvo": 4},
        "jogurt": {"cena": 0.60, "mnozstvo": 6},
        "maslo": {"cena": 2.50, "mnozstvo": 2},
        "syr": {"cena": 2.00, "mnozstvo": 3}
    },
    "pecivo": {
        "chlieb": {"cena": 1.80, "mnozstvo": 3},
        "rohlik": {"cena": 0.15, "mnozstvo": 20},
        "bageta": {"cena": 0.90, "mnozstvo": 5}
    }
}

zlavove_kupony = {
    "midzlava": 0.20,      
    "lowzlava": 0.10,   
    "peekzlava": 0.50      
}

nakupny_kosik = []

while True:
    print("Čo chcete pridať do košíka? ")
    vstup = input("Zadajte položku: ").strip().lower()
    
    if vstup == "uz nic" or vstup == "koniec":
        break
    
    najdena = False
    for kategoria, produkty in sklad.items():
        if vstup in produkty:
            najdena = True
            if produkty[vstup]["mnozstvo"] > 0:
                nakupny_kosik.append(vstup)
                produkty[vstup]["mnozstvo"] -= 1
                print(f"-> Máš pridanú položku. Zostáva ich už len: {produkty[vstup]['mnozstvo']} ks")
            else:
                print(f"-> Ľutujeme, položku '{vstup}' už vykúpili dôchodci.")
            break
            
    if not najdena:
        print(f"-> '{vstup}' nie je spawnutá na sklade.")

    print("\n----------------------------------------")

print("Všetky tvoje veci:")

celkova_suma = 0

for polozka in nakupny_kosik:
    for kategoria, produkty in sklad.items():
        if polozka in produkty:
            cena = produkty[polozka]["cena"]
            celkova_suma += cena
            print(f"- {polozka} ({kategoria}): {cena:.2f} €")

celkova_suma = round(celkova_suma, 2)

print("----------------------------------------")

zlava = 0
chce_kupon = input("Máte zľavový kupón? (ano/nie): ").strip().lower()

if chce_kupon == "ano":
    kod = input("Zadajte kód kupónu: ").strip().lower()
    
    if kod in zlavove_kupony:
        hodnota_kuponu = zlavove_kupony[kod]
        
        if hodnota_kuponu < 1:
            zlava = celkova_suma * hodnota_kuponu
            print(f"-> Úspešne uplatnený kupón! Zľava: {int(hodnota_kuponu * 100)}%")
        else:
            zlava = hodnota_kuponu
            print(f"-> Úspešne uplatnený kupón! Zľava: {zlava:.2f} €")
            
        celkova_suma -= zlava
        celkova_suma = round(celkova_suma, 2)
        
        if celkova_suma < 0:
            celkova_suma = 0
    else:
        print("-> Neplatný kupón. Pokračujeme bez zľavy.")


celkova_suma = round(round(celkova_suma / 0.05) * 0.05, 2)

print("----------------------------------------")
print(f"Zaplať, inak máš po chlebe: {celkova_suma:.2f} €")
