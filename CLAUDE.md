# Spring Music — Claude Session Notes

## Project Overview
Spring Music is a sample Cloud Foundry application built with Spring Boot that demonstrates multi-database support. The original codebase is Java/Gradle. This session added a Python/Flask equivalent and a standalone HTML frontend.

## Repository
- **GitHub:** https://github.com/aghilan-ma/spring-music.git
- **Also mirrored at:** https://github.com/aghilan-ma/Team-7-BDC7C

## Project Structure
```
spring-music-master/
├── app.py                  # Python/Flask port of the Spring Boot app
├── spring-music.html       # Standalone HTML frontend for the /albums API
├── src/
│   └── main/
│       ├── java/           # Original Java source (Spring Boot)
│       └── resources/
│           ├── albums.json         # Seed data (29 albums)
│           ├── application.yml     # Spring DB profiles config
│           └── static/             # Original built-in frontend (HTML/JS/CSS)
├── build.gradle            # Gradle build (requires Java 8+)
└── manifest.yml            # Cloud Foundry deployment manifest
```

## Python App (app.py)

### Dependencies
```bash
pip install flask flask-sqlalchemy flask-cors
```

> Note: On this machine, use the full Python path due to multiple Python installations:
> `C:/Users/aghilan.maheswaran/AppData/Local/Python/bin/python.exe`

### Running
```bash
# Default — in-memory SQLite
python app.py

# With MySQL
python app.py --profile mysql

# With Postgres
python app.py --profile postgres

# Via environment variable
SPRING_PROFILES_ACTIVE=mysql python app.py
```

Server starts on **http://localhost:8080**

### REST API Endpoints
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /albums | List all albums |
| GET | /albums/{id} | Get album by ID |
| PUT | /albums | Add new album |
| POST | /albums | Update existing album |
| DELETE | /albums/{id} | Delete album |
| GET | /appinfo | Active profiles & services |
| GET | /errors/throw | Simulate runtime error |
| GET | /errors/kill | Shutdown the app |

### Java → Python Mapping
| Java Class | Python equivalent |
|---|---|
| `Application.java` | `if __name__ == "__main__"` block |
| `Album.java` | `Album` SQLAlchemy model |
| `RandomIdGenerator.java` | `uuid.uuid4()` default |
| `AlbumController.java` | `/albums` Flask routes |
| `InfoController.java` | `/appinfo` route |
| `ErrorController.java` | `/errors/*` routes |
| `AlbumRepositoryPopulator.java` | `populate_albums()` function |
| `application.yml` | `PROFILES` dict + argparse |

## HTML Frontend (spring-music.html)
A standalone single-file frontend. Open directly in a browser — no server needed to view it.

- Set the API Base URL to `http://localhost:8080` and click **Connect**
- Features: sortable table, live search, Add/Edit/Delete modals, stats bar

## Environment Notes
- **OS:** Windows 11 Enterprise
- **Java:** Not installed — use Python app instead
- **Python:** 3.14.3 at `C:/Users/aghilan.maheswaran/AppData/Local/Python/bin/python.exe`
- **Flask:** 3.1.3 (installed in pythoncore-3.14-64)
