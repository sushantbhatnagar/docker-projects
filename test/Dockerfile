# From https://github.com/NYULibraries/selenium_chrome_headless_ruby
ARG RUBY_VERSION=3.1.3

FROM ruby:${RUBY_VERSION}
#
#ARG INSTALL_PATH=/e2e_tests
#RUN mkdir e2e_tests
#WORKDIR $INSTALL_PATH

RUN #touch Gemfile

# Install essentials
RUN apt-get update -qq && apt-get install -y --no-install-recommends build-essential dos2unix nano libpq-dev cmake\
&& rm -rf /var/lib/apt/lists/*

# Install Chrome
RUN echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" | \
tee -a /etc/apt/sources.list.d/google.list && \
wget wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | \
apt-key add - && \
apt-get update && \
apt-get install -y google-chrome-stable libxss1

# Install Chrome driver
RUN BROWSER_MAJOR=$(google-chrome --version | sed 's/Google Chrome \([0-9]*\).*/\1/g') && \
wget https://chromedriver.storage.googleapis.com/LATEST_RELEASE_${BROWSER_MAJOR} -O chrome_version && \
wget https://chromedriver.storage.googleapis.com/`cat chrome_version`/chromedriver_linux64.zip && \
unzip chromedriver_linux64.zip && \
mv chromedriver /usr/local/bin/ && \
DRIVER_MAJOR=$(chromedriver --version | sed 's/ChromeDriver \([0-9]*\).*/\1/g') && \
echo "chrome version: $BROWSER_MAJOR" && \
echo "chromedriver version: $DRIVER_MAJOR" && \
if [ $BROWSER_MAJOR != $DRIVER_MAJOR ]; then echo "VERSION MISMATCH"; exit 1; fi

#ENV TZ=America/New_York
#RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN gem install bundler
RUN bundle install

# Add test code
#ADD . $INSTALL_PATH