if get_option('gtk3')

if not get_option('install-gplugin-gtk-viewer')
summary('gplugin-gtk-viewer',
        'You have disabled installation of gplugin-gtk-viewer which is a ' +
        'very useful diagnostics tool.',
        section : 'Warnings')
endif

gplugin_gtk_viewer_resources = gnome.compile_resources(
	'gplugin-gtk-viewer-resources',
	'data/gplugin-gtk-viewer.gresource.xml',
	c_name: 'gplugin_gtk_viewer',
	source_dir: 'data')

gplugin_gtk_viewer = executable('gplugin-gtk-viewer',
	'gplugin-gtk-viewer.c',
	'gplugin-gtk-viewer-window.c',
	'gplugin-gtk-viewer-window.h',
	dependencies : [gplugin_dep, gplugin_gtk_dep],
	install : get_option('install-gplugin-gtk-viewer')
)

if get_option('help2man')
	gplugin_gtk_viewer_man = custom_target('gplugin-gtk-viewer.1',
		command : [help2man,
		           '--name=View installed plugins', '--section=1',
		           '--help-option=--help-all', '--no-info',
		           '--output', '@OUTPUT@',
		           gplugin_gtk_viewer],
		output : 'gplugin-gtk-viewer.1',
		install : get_option('install-gplugin-gtk-viewer'),
		install_dir : get_option('mandir') / 'man1')
endif

endif
