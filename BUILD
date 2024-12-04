# Copyright 2024 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
"""Bazel Rules for NASM

This file defines rules for using the NASM assembler.  It exposes a `nasm`
binary and library targets for building NASM itself, plus Starlark rules for
building with it.

The main rules for users are defined in `defs.bzl`:

- `nasm_compile`: Compiles `.asm` files into object files.
- `nasm_library`: Creates a static library from a collection of `.asm` files.


Example:

```starlark
load("@nasm/bazel:defs.bzl", "nasm_compile", "nasm_library")

nasm_compile(
    name = "my_asm_object",
    src = "my_asm_code.asm",
    out = "my_asm_code.o",
    output_format = "elf64",  # Or "win64", "macho64"
)

nasm_library(
    name = "my_asm_library",
    srcs = ["one.asm", "two.asm"],
    output_format = "elf64",
)

cc_binary(
    name = "my_program",
    srcs = ["main.c"],
    deps = [":my_asm_library"],
)
```

Library targets:

- nasm_lib: A cc_library containing the NASM code.
- nasm: A cc_binary that builds the nasm assembler.
"""

load("@rules_license//rules:license.bzl", "license")
load("@rules_license//rules:license_kind.bzl", "license_kind")

package(
    default_applicable_licenses = [":license"],
    default_visibility = ["//visibility:public"],
)

license(
    name = "license",
    license_kinds = [
        ":SPDX-license-identifier-BSD-2.0",
    ],
    visibility = [":__subpackages__"],
)

license_kind(
    name = "SPDX-license-identifier-BSD-2.0",
    conditions = ["notice"],
    url = "https://spdx.org/licenses/BSD-2-Clause.html",
)

INCLUDES = [
    "include",
    "x86",
    "asm",
    "disasm",
    "output",
]

COPTS = select({
    ":windows": [],
    "@platforms//os:windows": [],
    "//conditions:default": [
        "-w",
        "-DHAVE_CONFIG_H",
    ],
})

cc_library(
    name = "nasm_lib",
    srcs = [
        "asm/assemble.c",
        "asm/directbl.c",
        "asm/directiv.c",
        "asm/error.c",
        "asm/eval.c",
        "asm/exprdump.c",
        "asm/exprlib.c",
        "asm/float.c",
        "asm/labels.c",
        "asm/listing.c",
        "asm/parser.c",
        "asm/pptok.c",
        "asm/pragma.c",
        "asm/preproc.c",
        "asm/preproc-nop.c",
        "asm/quote.c",
        "asm/rdstrnum.c",
        "asm/segalloc.c",
        "asm/stdscan.c",
        "asm/strfunc.c",
        "asm/tokhash.c",
        "common/common.c",
        "disasm/disasm.c",
        "disasm/sync.c",
        "macros/macros.c",
        "nasmlib/badenum.c",
        "nasmlib/bsi.c",
        "nasmlib/crc64.c",
        "nasmlib/file.c",
        "nasmlib/filename.c",
        "nasmlib/hashtbl.c",
        "nasmlib/ilog2.c",
        "nasmlib/malloc.c",
        "nasmlib/md5c.c",
        "nasmlib/mmap.c",
        "nasmlib/path.c",
        "nasmlib/perfhash.c",
        "nasmlib/raa.c",
        "nasmlib/rbtree.c",
        "nasmlib/readnum.c",
        "nasmlib/realpath.c",
        "nasmlib/saa.c",
        "nasmlib/srcfile.c",
        "nasmlib/string.c",
        "nasmlib/strlist.c",
        "nasmlib/ver.c",
        "nasmlib/zerobuf.c",
        "output/codeview.c",
        "output/legacy.c",
        "output/nulldbg.c",
        "output/nullout.c",
        "output/outaout.c",
        "output/outas86.c",
        "output/outbin.c",
        "output/outcoff.c",
        "output/outdbg.c",
        "output/outelf.c",
        "output/outform.c",
        "output/outieee.c",
        "output/outlib.c",
        "output/outmacho.c",
        "output/outobj.c",
        "output/outrdf2.c",
        "output/strtbl.c",
        "stdlib/snprintf.c",
        "stdlib/strlcpy.c",
        "stdlib/strnlen.c",
        "stdlib/strrchrnul.c",
        "stdlib/vsnprintf.c",
        "x86/disp8.c",
        "x86/iflag.c",
        "x86/insnsa.c",
        "x86/insnsb.c",
        "x86/insnsd.c",
        "x86/insnsn.c",
        "x86/regdis.c",
        "x86/regflags.c",
        "x86/regs.c",
        "x86/regvals.c",
    ],
    hdrs = glob([
        "*.h",
        "include/*.h",
        "x86/*.h",
        "disasm/*.h",
        "config/*.h",
        "asm/*.h",
        "output/*.h",
        "nasmlib/*.h",
    ]),
    copts = COPTS,
    includes = INCLUDES,
    visibility = ["//visibility:private"],
)

cc_binary(
    name = "nasm",
    srcs = [
        "asm/nasm.c",
        "nasmlib/zerobuf.c",
    ],
    copts = COPTS,
    includes = INCLUDES,
    deps = [
        ":nasm_lib",
    ],
)

config_setting(
    name = "windows",
    values = {
        "cpu": "x64_windows",
    },
)
