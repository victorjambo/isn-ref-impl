# Installing Java

This Project uses Java 52.

## Steps

- You can use SDKMAN! (https://sdkman.io/) to install Java. Ensure you have sdk installed before you proceed.

  ```bash
  curl -s "https://get.sdkman.io" | bash
  sdk version
  ```

- Install Java 52.0

  ```bash
  sdk install java 8.0.382-zulu
  ```

- if that doesn't work try

  ```bash
  sdk install java 8.0.392-tem
  ```

- Confirm java is installed

  ```bash
  java -version
  ```

- Install clojure 👉 https://clojure.org/guides/install_clojure and confirm its installed

  ```bash
  clj --version
  ```

- Finally run the project

  ```bash
  ./run.sh
  ```

  or

  ```bash
  clj -X app.core/-main
  ```
