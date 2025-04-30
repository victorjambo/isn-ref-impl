# Running the project

This project fork comes with configs that should work out of the box.

## Steps

- This Project uses Java 52. You can use SDKMAN! (https://sdkman.io/) to install Java. Ensure you have sdk installed before you proceed.

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

- Update config file with location of your project https://github.com/victorjambo/isn-ref-impl/blob/develop/config.edn#L26 

- Create signals folder. its used to store sent signals

  ```bash
  mkdir signals
  ```

- Finally run the project

  ```bash
  ./run.sh
  ```

  or

  ```bash
  clj -X app.core/-main
  ```

- The frontend UI runs on port 5001. Access the dashboard from http://localhost:5001

- On the UI dashboard, click login and paste `https://victorjambo.github.io` in the input field.

- Enure your `.env.serve` file (In TWIN repo) has these variables. `BTD_FSA_ACCESS_TOKEN` and `BTD_PHA_ACCESS_TOKEN` can be copied from http://localhost:5001/account

  ```bash
  BTD_FSA_ENDPOINT=http://localhost:5001/micropub
  BTD_FSA_ACCESS_TOKEN=xxx
  BTD_PHA_ENDPOINT=http://localhost:5001/micropub-btd
  BTD_PHA_ACCESS_TOKEN=xxx
  ```

### Caveats

You need to create your own Github Pages for you to login with github. Follow steps bellow;


- Fork this repo https://github.com/victorjambo/victorjambo.github.io
- Update the name of the repo to something like your-profile-handle.github.io
- Update this line https://github.com/victorjambo/victorjambo.github.io/blob/main/index.html#L9 with your-profile-handle.github.io
- Update config.edn. Replace all traces of victorjambo.github.io with your-profile-handle.github.io
- On the UI, on the [login page](http://localhost:5001/login) paste the URL http://your-profile-handle.github.io
- Finally, on your github Profile add http://your-profile-handle.github.io as your website
