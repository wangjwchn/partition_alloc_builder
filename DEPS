# Copyright 2021 The Chromium Authors
# Use of this source code is governed by a BSD-style license that can be
# found in the LICENSE file.

# PartitionAlloc is planned to be extracted into a standalone library, and
# therefore dependencies need to be strictly controlled and minimized.

gclient_gn_args_file = 'partition_alloc_builder/build/config/gclient_args.gni'
gclient_gn_args = [
  'build_with_chromium',
]

# Only these hosts are allowed for dependencies in this DEPS file.
# This is a subset of chromium/src/DEPS's allowed_hosts.
allowed_hosts = [
  'chromium.googlesource.com',
]

vars = {
  # Variable that can be used to support multiple build scenarios, like having
  # Chromium specific targets in a client project's GN file or sync dependencies
  # conditionally etc.
  'build_with_chromium': False,

  'chromium_git': 'https://chromium.googlesource.com',
}

deps = {
  # https://chromium.googlesource.com/chromium/src/base/allocator/partition_allocator/+/f519950b1da2ff90149fb66cc80aef17ee4f846a
  'partition_alloc_builder/base/allocator/partition_allocator':
      Var('chromium_git') +
      '/chromium/src/base/allocator/partition_allocator.git@f519950b1da2ff90149fb66cc80aef17ee4f846a',
  # https://chromium.googlesource.com/chromium/src/build/+/a9b942d006818e3ce789916349343a69e25f5f7c
  'partition_alloc_builder/build':
      Var('chromium_git') + '/chromium/src/build.git@a9b942d006818e3ce789916349343a69e25f5f7c',
  # https://chromium.googlesource.com/chromium/src/build_overrides/+/9c6898f34730d9bd141a14149d498d77f387242a
  'partition_alloc_builder/build_overrides':
      Var('chromium_git') + '/chromium/src/build_overrides.git@9c6898f34730d9bd141a14149d498d77f387242a',
  'partition_alloc_builder/buildtools':
  # https://chromium.googlesource.com/chromium/src/buildtools/+/5d2ccbf93c7d956a9aff1d0acd21155e6b515ceb
      Var('chromium_git') + '/chromium/src/buildtools.git@5d2ccbf93c7d956a9aff1d0acd21155e6b515ceb',
  'partition_alloc_builder/buildtools/clang_format/script':
      Var('chromium_git') +
      '/external/github.com/llvm/llvm-project/clang/tools/clang-format.git',
  'partition_alloc_builder/buildtools/linux64': {
    'packages': [
      {
        'package': 'gn/gn/linux-${{arch}}',
        'version': 'latest',
      }
    ],
    'dep_type': 'cipd',
    'condition': 'host_os == "linux"',
  },
  'partition_alloc_builder/buildtools/mac': {
    'packages': [
      {
        'package': 'gn/gn/mac-${{arch}}',
        'version': 'latest',
      }
    ],
    'dep_type': 'cipd',
    'condition': 'host_os == "mac"',
  },
  'partition_alloc_builder/buildtools/win': {
    'packages': [
      {
        'package': 'gn/gn/windows-amd64',
        'version': 'latest',
      }
    ],
    'dep_type': 'cipd',
    'condition': 'host_os == "win"',
  },
    # https://chromium.googlesource.com/external/github.com/llvm/llvm-project/libcxx/+/3f3f7532e900f6703eaca3c1f6ea99542b6e2501
  'partition_alloc_builder/buildtools/third_party/libc++/trunk':
      Var('chromium_git') + '/external/github.com/llvm/llvm-project/libcxx.git@3f3f7532e900f6703eaca3c1f6ea99542b6e2501',
  # https://chromium.googlesource.com/external/github.com/llvm/llvm-project/libcxxabi/+/1a32724f721e1c3b6c590a07fe4a954344f15e48
  'partition_alloc_builder/buildtools/third_party/libc++abi/trunk':
      Var('chromium_git') +
      '/external/github.com/llvm/llvm-project/libcxxabi.git@1a32724f721e1c3b6c590a07fe4a954344f15e48',
  # https://chromium.googlesource.com/chromium/src/tools/clang/+/75ced899f080bc079060394dc93dbffba55383da
  'partition_alloc_builder/tools/clang':
      Var('chromium_git') + '/chromium/src/tools/clang.git@75ced899f080bc079060394dc93dbffba55383da',
  'partition_alloc_builder/testing':
      Var('chromium_git') + '/chromium/src/testing.git',
  'partition_alloc_builder/third_party/googletest/src':
      Var('chromium_git') + '/external/github.com/google/googletest.git',
  'partition_alloc_builder/third_party/lss': {
      'url': Var('chromium_git') + '/linux-syscall-support.git',
      'condition': 'checkout_android or checkout_linux',
  },
  'partition_alloc_builder/third_party/ninja': {
    'packages': [
      {
        'package': 'infra/3pp/tools/ninja/${{platform}}',
        'version': 'latest',
      }
    ],
    'dep_type': 'cipd',
  },
}

hooks = [
  {
    'name': 'sysroot_arm',
    'pattern': '.',
    'condition': 'checkout_linux and checkout_arm',
    'action': [
        'python3',
        'partition_alloc_builder/build/linux/sysroot_scripts/install-sysroot.py',
        '--arch=arm'],
  },
  {
    'name': 'sysroot_arm64',
    'pattern': '.',
    'condition': 'checkout_linux and checkout_arm64',
    'action': [
        'python3',
        'partition_alloc_builder/build/linux/sysroot_scripts/install-sysroot.py',
        '--arch=arm64'],
  },
  {
    'name': 'sysroot_x86',
    'pattern': '.',
    'condition': 'checkout_linux and (checkout_x86 or checkout_x64)',
    'action': [
        'python3',
        'partition_alloc_builder/build/linux/sysroot_scripts/install-sysroot.py',
        '--arch=x86'],
  },
  {
    'name': 'sysroot_mips',
    'pattern': '.',
    'condition': 'checkout_linux and checkout_mips',
    'action': [
        'python3',
        'partition_alloc_builder/build/linux/sysroot_scripts/install-sysroot.py',
        '--arch=mips'],
  },
  {
    'name': 'sysroot_mips64',
    'pattern': '.',
    'condition': 'checkout_linux and checkout_mips64',
    'action': [
        'python3',
        'partition_alloc_builder/build/linux/sysroot_scripts/install-sysroot.py',
        '--arch=mips64el'],
  },
  {
    'name': 'sysroot_x64',
    'pattern': '.',
    'condition': 'checkout_linux and checkout_x64',
    'action': [
        'python3',
        'partition_alloc_builder/build/linux/sysroot_scripts/install-sysroot.py',
        '--arch=x64'],
  },
  {
    # Update the prebuilt clang toolchain.
    # Note: On Win, this should run after win_toolchain, as it may use it.
    'name': 'clang',
    'pattern': '.',
    'action': [
        'python3',
        'partition_alloc_builder/tools/clang/scripts/update.py'],
  },
]