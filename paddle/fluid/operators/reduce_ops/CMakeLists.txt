include(operators)
if(WITH_UNITY_BUILD)
  # Load Unity Build rules for operators in paddle/fluid/operators/reduce_ops.
  include(unity_build_rule.cmake)
endif()
if(WITH_GPU)
  if(${CMAKE_CUDA_COMPILER_VERSION} LESS 11.0)
    register_operators(DEPS cub)
  else()
    register_operators()
  endif()
else()
  register_operators()
endif()

if(WITH_GPU)
  if(${CMAKE_CUDA_COMPILER_VERSION} LESS 11.0)
    nv_test(
      check_reduce_rank_test
      SRCS check_reduce_rank_test.cu
      DEPS tensor cub)
  else()
    nv_test(
      check_reduce_rank_test
      SRCS check_reduce_rank_test.cu
      DEPS tensor)
  endif()
endif()

if(WITH_ROCM)
  hip_test(
    check_reduce_rank_test
    SRCS check_reduce_rank_test.cu
    DEPS tensor)
endif()
