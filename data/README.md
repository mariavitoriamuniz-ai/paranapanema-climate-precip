# Data

The analysis uses three files of **simulated** daily precipitation averaged over the UGRH Paranapanema basin, provided to students during the *Mudanças Climáticas* course at UFF. They are not redistributed in this repository.

| File | Period | Content |
|---|---|---|
| `paranapanema-hist.parquet` | 1850–1919 | Early-historical simulation, used as the reference climatology |
| `paranapanema-20c.parquet` | 1920–2005 | 20th-century simulation, 40 ensemble members |
| `paranapanema-rcp85.parquet` | 2006–2100 | RCP8.5 projection, same 40 members |

Main columns: `time` (date), `member` (ensemble member, 20C and RCP8.5), `mean_mm_day` (basin-mean precipitation, mm/day).

All three datasets come from a global climate model of the CMIP5 generation; none of them are observations.

The notebook expects the files in `MyDrive/Mudanças Climáticas/` on Google Drive.
