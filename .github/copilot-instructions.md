## Repo summary

- This is a small personal portfolio site (static HTML/CSS) plus a single Java JDBC demo (`jdbcmaking.java`).
- Frontend: `index.html`, `style.css`, images referenced from the root (e.g. `portfolioimg.jpg`) and a `pictures/` directory.
- Backend/demo: `jdbcmaking.java` is a standalone Java program demonstrating connecting to a local MySQL database and performing a simple insert/query.

## What an AI coding agent should know up front

- No build system (Maven/Gradle/npm) is present. Changes are applied directly to source files and compiled/run with the platform tools.
- Database integration is local: `jdbcmaking.java` uses com.mysql.cj.jdbc.Driver and connects to `jdbc:mysql://localhost:3306/testdb` with username/password `root`/`root` (hard-coded). See `jdbcmaking.java` lines where DriverManager is invoked.
- Static site files are plain HTML/CSS. Opening `index.html` in a browser is sufficient to preview changes. A simple static server (e.g., `python -m http.server`) is acceptable during testing.

## Important files to reference

- `jdbcmaking.java` — JDBC demo. Key calls:
  - `Class.forName("com.mysql.cj.jdbc.Driver")`
  - `DriverManager.getConnection("jdbc:mysql://localhost:3306/testdb", "root", "root")`
  - `insert into student values(?,?,?)` and `select * from student`
- `index.html` — main static page; references `portfolioimg.jpg` (note: path is relative to repo root).
- `style.css` — styles for the static site.

## Developer workflows / run commands (Windows PowerShell)

- Preview site: open `index.html` in a browser or run a local server in repo root:

  ```powershell
  # from repository root
  python -m http.server 8000
  # then open http://localhost:8000/index.html
  ```

- Compile & run Java demo (requires MySQL Connector/J jar and a running MySQL server with `testdb`):

  1) Ensure the connector jar (e.g. `mysql-connector-java-8.0.xx.jar`) is downloaded and note its path.
  2) Compile:
     ```powershell
     javac jdbcmaking.java
     ```
  3) Run (replace path-to-jar):
     ```powershell
     java -cp ".;C:\path\to\mysql-connector-java-8.0.xx.jar" jdbcmaking
     ```

  4) Database bootstrap (if needed):
     ```sql
     CREATE DATABASE testdb;
     USE testdb;
     CREATE TABLE student(id INT PRIMARY KEY, name VARCHAR(100), marks INT);
     ```

## Repository-specific conventions & gotchas

- Single-file Java demo uses no packages and a lowercase class name (`jdbcmaking`) — keep filename and class name consistent when renaming.
- Credentials are hard-coded in `jdbcmaking.java` (root/root). Treat this as a local demo only; do not assume these credentials exist on target machines. Prefer advising use of environment variables or a config file when making changes.
- `index.html` references `portfolioimg.jpg` from the repository root; if images are stored in `pictures/`, update paths or move the image. Verify asset paths when editing HTML.

## Integration points & external dependencies

- MySQL server running locally (port 3306) and the MySQL Connector/J jar on the Java classpath are required to run `jdbcmaking.java`.
- No CI, no package manifest, and no automated tests are present in the repo.

## Practical editing guidance for an AI

- When modifying `jdbcmaking.java`:
  - Preserve the JDBC URL pattern and clearly surface database credentials via variables if you change them.
  - If you add external Java dependencies, recommend migrating to Maven/Gradle and include a sample `pom.xml`/`build.gradle` rather than inlining jars in instructions.

- When modifying `index.html`/assets:
  - Keep paths relative to repository root as currently used, or update all references consistently.
  - Run a local static server to validate changes in a browser.

## Safety and quick checks

- Do not commit production credentials. If you find secrets, flag them to the user.
- Quick sanity checks an agent should run before committing:
  - `javac` compilation for Java edits.
  - Open `index.html` or run a local server to ensure assets load.
  - If touching JDBC code, verify DB connectivity steps are documented or add a brief bootstrap SQL as shown above.

---

If anything important is missing (for example, where the project's images are stored or preferred DB credentials), tell me what you'd like clarified and I will update this doc.
