summary('python3', get_option('python3'), section : 'Loader Support', bool_yn : true)

if get_option('python3')
	if not get_option('introspection')
		error('Python3 plugin requires GObject Introspection.')
	endif

	GPLUGIN_PYTHON3_SOURCES = [
		'gplugin-python3-core.c',
		'gplugin-python3-loader.c',
		'gplugin-python3-plugin.c',
		'gplugin-python3-utils.c',
	]

	GPLUGIN_PYTHON3_HEADERS = [
		'gplugin-python3-loader.h',
		'gplugin-python3-plugin.h',
		'gplugin-python3-utils.h',
	]

	PYGOBJECT = dependency('pygobject-3.0', version: '>=3.0.0')

	_PYTHONS = [
		['python-3.10-embed', '>=3.10'],
		['python-3.9-embed', '>=3.9'],
		['python-3.8-embed', '>=3.8'],
		['python3-embed', '>=3.8'],
		['python3', '>=3.8'],
		['python-3.7', '>=3.7'],
		['python-3.7m', '>=3.7'],
		['python-3.6', '>=3.6'],
		['python-3.6m', '>=3.6'],
	]
	PYTHON_FOUND = false
	PYGOBJECT_WORKS = false
	foreach _PYTHON : _PYTHONS
		PYTHON3 = dependency(_PYTHON[0], version : _PYTHON[1], required : false)
		if PYTHON3.found()
			PYTHON_FOUND = true

			python3_gi_test = compiler.run(files('gplugin-python3-test-pygobject.c'),
				dependencies : [GLIB, PYTHON3, PYGOBJECT],
				name : 'Python3 GI')
			if not python3_gi_test.compiled() or python3_gi_test.returncode() != 0
				message('pygobject does not work with @0@'.format(_PYTHON[0]))
				continue
			endif

			PYGOBJECT_WORKS = true
			break
		endif
	endforeach

	if not PYTHON_FOUND
		error('failed to find a usable python')
	endif

	if not PYGOBJECT_WORKS
		error('found a usable python but pygobject does not work with it')
	endif

	# Now add our libraries
	gplugin_python3_inc = include_directories('.')
	gplugin_python3 = shared_library('gplugin-python3',
		GPLUGIN_PYTHON3_SOURCES,
		GPLUGIN_PYTHON3_HEADERS,
		name_prefix : '',
		dependencies : [PYTHON3, PYGOBJECT, gplugin_dep],
		install : true,
		install_dir : get_option('libdir') / 'gplugin'
	)
	gplugin_python3_dep = declare_dependency(
		include_directories : gplugin_python3_inc,
		link_with : gplugin_python3,
	)

	gplugin_python3_static = static_library('gplugin-python3-static',
		GPLUGIN_PYTHON3_SOURCES,
		GPLUGIN_PYTHON3_HEADERS,
		dependencies : [PYTHON3, PYGOBJECT, gplugin_dep],
	)
	gplugin_python3_static_dep = declare_dependency(
		include_directories : gplugin_python3_inc,
		link_with : gplugin_python3_static,
	)
endif # python3

subdir('tests')
