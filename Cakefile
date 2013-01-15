fs = require 'fs'
_ = require 'underscore'
watch = require 'watch'

TEMPLATE_SRC = "#{ __dirname }/templates"
TEMPLATE_OUTPUT = "#{ ROOT_SRC }/templates.coffee"
TEMPLATE_SEPARATOR = '-'

exportify = (f) ->
  # Repackage template with exports syntax
  templateName = f.replace '.html', ''
  templateExportName = templateName.replace TEMPLATE_SEPARATOR, '.'
  templateFilePath = "#{ TEMPLATE_SRC }/#{ f }"
  body = fs.readFileSync templateFilePath, 'utf-8'
  content = "exports.#{ templateExportName } = \"\"\"#{ body }\"\"\""

buildTemplate = ->
  # Get list of template files
  files = fs.readdirSync TEMPLATE_SRC
  # Get list of object keys to init
  keys = (f.split('.')[0].split('-')[0] for f in files)
  keyBlocks = ("exports.#{ key } = {}" for key in _.uniq(keys))
  # Map files to list of exports.*
  templateBlocks = (exportify f for f in files)
  # Concatentate templates into a single file
  content = '# TEMPLATES.COFFEE IS AUTO-GENERATED. CHANGES WILL BE LOST!\n'
  content += keyBlocks.join('\n') + '\n\n'
  content += templateBlocks.join '\n\n'
  fs.writeFileSync TEMPLATE_OUTPUT, content, 'utf-8'
  console.log "#{ new Date().toString() } - #{ Buffer(content).length } bytes written"

task 'templates', "Compiles/concatentates templates/*.html to src/templates.coffee", ->
  console.log "Writing to #{ TEMPLATE_OUTPUT }"
  buildTemplate()

task 'templates-watch', "Watch for changes in templates/*.html", ->
  console.log "Watching #{ TEMPLATE_SRC }/*.html"
  watch.watchTree TEMPLATE_SRC, buildTemplate