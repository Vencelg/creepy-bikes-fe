## Installation and Setup

### Prerequisites

- **NodeJS**
- **NPM**

### Local setup

1. **Clone the repository**:
    ```bash
    git clone https://github.com/Vencelg/creepy-bikes-fe.git
    cd creepy-bikes-fe
    ```

2. **Copy the `.env.example` file to `.env`**:
    ```bash
    cp .env.example .env
    ```

3. **Fill `VITE_GOOGLE_MAPS_API_KEY` attribute in `.env` file**:
    ```bash
    VITE_GOOGLE_MAPS_API_KEY="your-api-key"
    ```
   
4. **Install dependencies**:
    ```bash
    npm i
    ````

5. **Start the application**:
    ```bash
    npm run dev
    ```

6. **Access the application**:
    - Visit `http://localhost:5173/`.


## Informace ke grafu
    U implementace grafu jsem narazil na problém s nedostatkem dat.
    Pokud jsem správně pochopil graf měl zobrazit počet kol na hodinu dne.
    Ovšem API umožňuje zobrazit data pouze v současnosti, čímž pádem nemám data zbytku dne.
    Momentálně se v aplikaci do grafu dají náhodná data, graf jsem naimplementoval, abych dokázal že to zvládnu.
    Jediné řešení, které mě napadlo, je v průběhu dne data ukládat, ovšem aplikace by musela být zapla tak na 8 hodin,
    aby se data nasbírala.

