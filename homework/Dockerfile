# Použití BUILDER STAGE
FROM python:3.12.3 AS builder

# Nastavení pracovního adresáře uvnitř kontejneru
WORKDIR /app

# Zkopíruju soubor requirements.txt z adresáře, ve kterém se aktuálně nacházím (zajistí tečka na konci)
COPY requirements.txt .

# Provede instalaci knihoven ze souboru requirements.txt
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt 


#######################################################################################################################


# Použití základního BASE IMAGE (Python3-slim)
FROM python:3.12.3-slim

WORKDIR /app

# Zkopírování nainstalovaných balíčků z builder stage, ale jen potřebných částí
COPY --from=builder /install /usr/local

# Zkopírování app.py souboru (případně lze použít 2 tečky, to zkopíruje vše z aktuálního adresáře) do kontejneru
COPY . .

# Vytvoření nového uživatele "appuser" ; -m dá domovský adresář userovi; chown změní vlastníka souboru; -R rekurzivně (na složku a její obsah)
RUN useradd -m appuser && chown -R appuser /app

# Přepnutí na "appuser" -> neběží jako root 
USER appuser

# Spustíme Python a app.py
#CMD ["python3", "app.py", "--help"]

# Spustím za pomocí ENTRYPOINT (například nadefinuji python3, s tím, že při spuštění docker run si definuji už jen spuštění konkrétního skriptu - samozřejmě .py)
ENTRYPOINT ["python3"]
# docker run --rm --name runentry image app.py 