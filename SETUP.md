# Fixing `bundle install` on macOS

The site uses the `github-pages` gem, which depends on several gems that need to **compile native code**. macOS system Ruby (`/usr/bin/ruby`) often fails to build these on newer macOS versions, with errors like:

- `No rule to make target ... ruby/config.h`
- `The compiler failed to generate an executable file`

**Fix: use a Ruby that isn’t the system one** (e.g. Homebrew or rbenv). Then `bundle install` and `bundle exec jekyll build` will work.

---

## Option A: Ruby via Homebrew (recommended)

1. **Install Homebrew** (if needed): https://brew.sh

2. **Install Ruby:**
   ```bash
   brew install ruby
   ```

3. **Use Homebrew’s Ruby** (add to your `~/.zshrc` so it’s used in new terminals):
   ```bash
   export PATH="/opt/homebrew/opt/ruby/bin:$PATH"
   export PATH="$(brew --prefix ruby)/lib/ruby/gems/$(ruby -e 'puts RUBY_VERSION')/bin:$PATH"
   ```
   Then run `source ~/.zshrc` or open a new terminal.

4. **Confirm you’re not using system Ruby:**
   ```bash
   which ruby   # should be /opt/homebrew/opt/ruby/bin/ruby or similar, not /usr/bin/ruby
   ruby --version
   ```

5. **Install bundler and project gems:**
   ```bash
   cd /path/to/xiangyum.github.io
   gem install bundler
   bundle config set --local path 'vendor/bundle'
   bundle install
   ```

6. **Build/serve the site:**
   ```bash
   bundle exec jekyll build
   # or
   bundle exec jekyll serve
   ```

---

## Option B: Ruby via rbenv

1. **Install rbenv and ruby-build:**
   ```bash
   brew install rbenv ruby-build
   rbenv init
   # follow the prompt to add rbenv to your shell (e.g. ~/.zshrc)
   ```

2. **Install a Ruby and use it in this project:**
   ```bash
   cd /path/to/xiangyum.github.io
   rbenv install 3.2.0
   rbenv local 3.2.0
   ```

3. **Install gems and build:**
   ```bash
   gem install bundler
   bundle config set --local path 'vendor/bundle'
   bundle install
   bundle exec jekyll build
   ```

---

## If you only need to deploy (e.g. GitHub Pages)

You don’t have to run Jekyll on your Mac. Push your branch to GitHub; GitHub Pages will run `bundle install` and `jekyll build` in its own environment. Your font and content changes will apply there. Local `bundle install` is only needed if you want to build or serve the site on your machine.
