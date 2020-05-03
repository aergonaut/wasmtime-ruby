# frozen_string_literal: true

require 'bundler/setup'
require 'rspec/core/rake_task'

import 'lib/tasks/build.rake'

desc 'Build example WASM modules'
task :wasm do
  cd 'wasm/fibonacci/'
  sh 'cargo build --target wasm32-unknown-unknown --release'
  cp 'target/wasm32-unknown-unknown/release/fibonacci.wasm', '../'

  cd '../markdown/'
  sh 'WASM_INTERFACE_TYPES=1 wasm-pack build'
  cp 'pkg/markdown.wasm', '../'

  cd '../types/'
  # sh 'WASM_INTERFACE_TYPES=1 wasm-pack build'
  sh 'wasm-pack build'
  # cp 'pkg/types.wasm', '../'
  cp 'pkg/types_bg.wasm', '../types.wasm'

  cd '../../'
end

desc 'Format sources'
task :format do
  sh 'cargo fmt'
  sh 'bundle',
     'exec',
     'rbprettier',
     '--write',
     '**/*.{rb,rake,gemspec}',
     '**/{Rakefile,Gemfile}'
end

RSpec::Core::RakeTask.new(:spec)

task spec: %i[wasm build]
task default: :spec
