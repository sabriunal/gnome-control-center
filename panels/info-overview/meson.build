panels_list += cappletname
desktop = 'gnome-@0@-panel.desktop'.format(cappletname)

desktop_in = configure_file(
  input: desktop + '.in.in',
  output: desktop + '.in',
  configuration: desktop_conf
)

i18n.merge_file(
  desktop,
  type: 'desktop',
  input: desktop_in,
  output: desktop,
  po_dir: po_dir,
  install: true,
  install_dir: control_center_desktopdir
)

cflags += [
  '-DBINDIR="@0@"'.format(control_center_bindir),
  '-DDATADIR="@0@"'.format(control_center_datadir),
  '-DGNOME_SESSION_DIR="@0@"'.format(gnome_session_libexecdir)
]

sources = files(
  'cc-info-overview-panel.c',
  'info-cleanup.c'
)

resource_data = files(
  'GnomeLogoVerticalMedium.svg',
  'cc-info-overview-panel.ui'
)

sources += gnome.compile_resources(
  'cc-' + cappletname + '-resources',
  cappletname + '.gresource.xml',
  c_name: 'cc_' + cappletname.underscorify (),
  dependencies: resource_data,
  export: true
)

deps = common_deps + [
  dependency('udisks2', version: '>= 2.1.8'),
  dependency('libgtop-2.0')
]

info_panel_lib = static_library(
  cappletname,
  sources: sources,
  include_directories: [ top_inc, common_inc ],
  dependencies: deps,
  c_args: cflags
)
panels_libs += info_panel_lib