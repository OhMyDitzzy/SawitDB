# SawitDB

![SawitDB Banner](docs/sawitdb.jpg)


**SawitDB** is a unique database solution stored in `.sawit` binary files.

The system features a custom **Paged Heap File** architecture similar to SQLite, using fixed-size 4KB pages to ensure efficient memory usage. What differentiates SawitDB is its unique **Agricultural Query Language (AQL)**, which replaces standard SQL keywords with Indonesian farming terminology.

**Now with Network Edition v2.0!** Connect via TCP using `sawitdb://` protocol similar to MongoDB.

**🚨 Emergency: Aceh Flood Relief**
Please support our brothers and sisters in Aceh.

[![Kitabisa](https://img.shields.io/badge/Kitabisa-Bantu%20Aceh-blue?style=flat&logo=heart)](https://kitabisa.com/campaign/donasipedulibanjiraceh)

*Organized by Human Initiative Aceh*

## Features

- **Paged Architecture**: Data is stored in 4096-byte binary pages. The engine does not load the entire database into memory.
- **Single File Storage**: All data, schema, and indexes are stored in a single `.sawit` file.
- **High Stability**: Uses 4KB atomic pages. More stable than a coalition government.
- **Data Integrity (Anti-Korupsi)**: Implements strict `fsync` protocols. Data cannot be "corrupted" or "disappear" mysteriously like social aid funds (Bansos). No "Sunat Massal" here.
- **Zero Bureaucracy (Zero Deps)**: Built entirely with standard Node.js. No unnecessary "Vendor Pengadaan" or "Mark-up Anggaran".
- **Transparansi**: Query language is clear. No "Pasal Karet" (Ambiguous Laws) or "Rapat Tertutup" in 5-star hotels.
- **Speed**: Faster than printing an e-KTP at the Kelurahan.
- **Network Support (NEW)**: Client-Server architecture with Multi-database support and Authentication.

## Filosofi

### Filosofi (ID)
SawitDB dibangun dengan semangat "Kemandirian Data". Kami percaya database yang handal tidak butuh **Infrastruktur Langit** yang harganya triliunan tapi sering *down*. Berbeda dengan proyek negara yang mahal di *budget* tapi murah di kualitas, SawitDB menggunakan arsitektur **Single File** (`.sawit`) yang hemat biaya. Backup cukup *copy-paste*, tidak perlu sewa vendor konsultan asing. Fitur **`fsync`** kami menjamin data tertulis di *disk*, karena bagi kami, integritas data adalah harga mati, bukan sekadar bahan konferensi pers untuk minta maaf.

### Philosophy (EN)
SawitDB is built with the spirit of "Data Sovereignty". We believe a reliable database doesn't need **"Sky Infrastructure"** that costs trillions yet goes *down* often. Unlike state projects that are expensive in budget but cheap in quality, SawitDB uses a cost-effective **Single File** (`.sawit`) architecture. Backup is just *copy-paste*, no need to hire expensive foreign consultants. Our **`fsync`** feature guarantees data is written to *disk*, because for us, data integrity is non-negotiable, not just material for a press conference to apologize.

## File List

- `src/WowoEngine.js`: Core Database Engine (Class: `SawitDB`).
- `bin/sawit-server.js`: Server executable.
- `cli/local.js`: Interactive CLI tool (Local).
- `cli/remote.js`: Interactive CLI tool (Network).
- `examples/`: Sample scripts.

## Installation

Ensure you have Node.js installed. Clone the repository.

```bash
# Clone
git clone https://github.com/WowoEngine/SawitDB.git
```

## Quick Start (Network Edition)

### 1. Start the Server
```bash
node bin/sawit-server.js
```
The server will start on `0.0.0.0:7878` by default.

**Environment Variables:**
```bash
SAWIT_PORT=7878           # Port to listen on
SAWIT_HOST=0.0.0.0        # Host to bind to
SAWIT_DATA_DIR=./data     # Directory for .sawit files
SAWIT_AUTH=user:pass      # Enable authentication (optional)
```

### 2. Connect with Client

**Option A: Interactive CLI**
```bash
node cli/remote.js sawitdb://localhost:7878/mydb
```

**Option B: Programmatic Usage**
See [Client API](#client-api) section.

## Usage (Embedded/Local)

### Initialization

```javascript
const SawitDB = require('./src/WowoEngine');
const path = require('path');

// Initialize the engine. File created automatically.
const db = new SawitDB(path.join(__dirname, 'plantation.sawit'));
```

### Executing Queries

Use the `.query()` method to execute strings written in Agricultural Query Language.

```javascript
// Create a new Table
const result = db.query("LAHAN oil_palm_a");
console.log(result); 
```

## Query Syntax (AQL)

SawitDB uses a strict syntax mapping standard database operations to farming metaphors.

### 1. Management Commands

#### Create Table (`LAHAN`)
Opens a new land (table) for planting.
```sql
LAHAN [table_name]
```
*Example:* `LAHAN sawit_blok_a`

#### Show Tables (`LIHAT`)
Surveys the land to see all opened tables.
```sql
LIHAT LAHAN
```

#### Drop Table (`BAKAR`)
Burns the land (deletes the table and all data). **Warning: Irreversible.**
```sql
BAKAR LAHAN [table_name]
```
*Example:* `BAKAR LAHAN sawit_blok_a`

### 2. Data Manipulation

#### Insert Data (`TANAM`)
Plants seeds (inserts records) into the land.
```sql
TANAM KE [table_name] (col1, col2, ...) BIBIT (val1, val2, ...)
```
*Example:* `TANAM KE sawit (id, bibit, umur) BIBIT (1, 'Tenera', 5)`

#### Select Data (`PANEN`)
Harvests (selects) data from the land.
- **Operators supported**: `=`, `!=`, `>`, `<`, `>=`, `<=`
- **Wildcard**: Use `*` to select all columns.
- **Conditions**: Support `AND` / `OR`.

```sql
PANEN * DARI [table_name]
PANEN [col1, col2] DARI [table_name]
PANEN * DARI [table_name] DIMANA [key] [op] [value]
```
*Example:* `PANEN * DARI sawit DIMANA umur > 3 AND jenis = 'Tenera'`

#### Update Data (`PUPUK`)
Fertilizes (updates) existing crops.
```sql
PUPUK [table_name] DENGAN [key]=[val], ... DIMANA [key] [op] [val]
```
*Example:* `PUPUK sawit DENGAN status='Panen Raya', yield=100 DIMANA umur >= 10`

#### Delete Data (`GUSUR`)
Evicts (deletes) crops from the land.
```sql
GUSUR DARI [table_name] DIMANA [key] [op] [val]
```
*Example:* `GUSUR DARI sawit DIMANA id = 99`

### 3. Advanced Features (v2.0)

#### Indexing (`INDEKS`)
Create B-Tree indexes for faster lookups (O(log n)).
```sql
INDEKS [table] PADA [field]
LIHAT INDEKS [table]
```

#### Aggregation (`HITUNG`)
Calculate statistics on your harvest.
```sql
HITUNG COUNT(*) DARI [table]
HITUNG SUM(field) DARI [table]
HITUNG AVG(field) DARI [table] KELOMPOK [group_field]
```

## Client API

```javascript
const SawitClient = require('./src/SawitClient');

// Connection String: sawitdb://[user:pass@]host:port/database
const client = new SawitClient('sawitdb://localhost:7878/plantation');

await client.connect();

// Execute queries
const result = await client.query('PANEN * DARI sawit');

// Switch database
await client.use('new_db');

client.disconnect();
```

## CLI Tool

### Local
```bash
node cli/local.js
```

### Remote
```bash
node cli/remote.js sawitdb://localhost:7878/plantation
```

## Architecture Details

- **Page 0 (Master Page)**:  Contains the file header (Magic bytes `WOWO`) and the Table Directory.
- **Table Directory**: Maps table names to their `Start Page ID` and `Last Page ID`.
- **Data Pages**: Each table is stored as a linked list of pages. Each page contains a header pointing to the next page, allowing the database to grow dynamically.
- **B-Tree Indexing**: Auxiliary structures for fast data retrieval.

## Performance

Benchmark results on standard hardware (5000 records):

| Operation | Speed | Time (Total) | Example |
|-----------|-------|:------------:|---------|
| **Insert (TANAM)** | ~34,194 ops/sec | 0.15s | 5000 inserts |
| **Select All (PANEN)** | ~7,450,454 ops/sec | 0.001s | Scan 5000 records |
| **Select Where** | < 0.001s / query | - | Full Scan |
| **Select w/ Index** | < 0.001s / query | - | Indexed Lookup |
| **Update (PUPUK)** | ~69,590 ops/sec | 0.014s | 1000 updates |
| **Delete (GUSUR)** | ~35,777 ops/sec | 0.028s | 1000 deletes |

*Note: Update and Delete are slower because they currently require a full linear scan of the table pages and a "Delete+Insert" strategy for updates to handle variable-length records safely.*

*Data Size: 188KB for 5000 records.*

<!-- ## Support Developer
- [![Saweria](https://img.shields.io/badge/Saweria-Support%20Me-orange?style=flat&logo=ko-fi)](https://saweria.co/patradev)

- **BTC**: `12EnneEriimQey3cqvxtv4ZUbvpmEbDinL`
- **BNB Smart Chain (BEP20)**: `0x471a58a2b5072cb50e3761dba3e15d19f080bdbc`
- **DOGE**: `DHrFZW6w9akaWuf8BCBGxxRLR3PegKTggF` -->
