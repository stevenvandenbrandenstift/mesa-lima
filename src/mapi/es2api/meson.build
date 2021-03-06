# Copyright © 2017 Intel Corporation

# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:

# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.

# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

es2_glapi_mapi_tmp_h = custom_target(
  'es2_glapi_mapi_tmp.h',
  input : ['../mapi_abi.py', gl_and_es_api_files],
  output : 'glapi_mapi_tmp.h',
  command : [prog_python2, '@INPUT0@', '--printer', 'es2api', '@INPUT1@'],
  depend_files : api_xml_files,
  capture : true,
)

libgles2 = shared_library(
  'GLESv2',
  ['../entry.c', es2_glapi_mapi_tmp_h],
  c_args : [c_msvc_compat_args, c_vis_args, '-DMAPI_MODE_BRIDGE',
            '-DMAPI_ABI_HEADER="@0@"'.format(es2_glapi_mapi_tmp_h.full_path())],
  link_args : [ld_args_gc_sections],
  include_directories : [inc_src, inc_include, inc_mapi],
  link_with : libglapi,
  dependencies : [dep_thread, dep_libdrm, dep_m, dep_dl],
  version : '2',
  install : true,
)

pkg.generate(
  name : 'glesv2',
  filebase : 'glesv2',
  description : 'Mesa OpenGL ES 2.0 library',
  version : meson.project_version(),
  libraries : libgles2,
  libraries_private : '-lm -ldl -lpthread -pthread',
)

if with_tests
  test('es2-ABI-check', find_program('ABI-check'))
endif
