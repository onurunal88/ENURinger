file(GLOB HEADERS "include/enumivo/chain/*.hpp" "include/enumivo/chain/contracts/*.hpp")

## SORT .cpp by most likely to change / break compile
add_library( enumivo_chain
             chain_config.cpp
             merkle.cpp
             name.cpp
             transaction.cpp
             block.cpp
             block_trace.cpp
             wast_to_wasm.cpp
             wasm_interface.cpp
             wasm_enumivo_validation.cpp
             wasm_enumivo_injection.cpp
             apply_context.cpp
             resource_limits.cpp

             fork_database.cpp
             get_config.cpp
             block_log.cpp
             asset.cpp


             global_property_object.cpp
             chain_controller.cpp

             contracts/enumivo_contract.cpp
             contracts/chain_initializer.cpp
             contracts/genesis_state.cpp
             contracts/abi_serializer.cpp

             webassembly/wavm.cpp
             webassembly/binaryen.cpp

             ${HEADERS}
             transaction_metadata.cpp)

target_link_libraries( enumivo_chain enu_utilities fc chainbase Logging IR WAST WASM Runtime
      wasm asmjs passes cfg ast emscripten-optimizer support softfloat 
                     )
target_include_directories( enumivo_chain
                            PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include" "${CMAKE_CURRENT_BINARY_DIR}/include"
                                   "${CMAKE_CURRENT_SOURCE_DIR}/../wasm-jit/Include"
                                   "${CMAKE_CURRENT_SOURCE_DIR}/../../externals/binaryen/src"
                            )

if(MSVC)
  set_source_files_properties( db_init.cpp db_block.cpp database.cpp block_log.cpp PROPERTIES COMPILE_FLAGS "/bigobj" )
endif(MSVC)


