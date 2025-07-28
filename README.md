# Google Test 使い方ガイド

## 📌 1. Google Testとは？

Google Test（gtest）は、Googleが開発したC++向けのユニットテストフレームワークです。  
関数やクラスが意図通りに動作しているかを自動で検証するために使用されます。

---

## 🧭 2. 基本的な使い方

```cpp
#include <gtest/gtest.h>

// テスト対象の関数
int add(int a, int b) {
    return a + b;
}

// テストケース
TEST(MyTestSuite, AddFunction) {
    EXPECT_EQ(add(2, 3), 5);
    EXPECT_EQ(add(-1, 1), 0);
    EXPECT_EQ(add(2, 2), 5);  // 失敗例
}
🏗️ 3. CMakeによるビルド方法
🔧 CMakeLists.txt 例
cmake
cmake_minimum_required(VERSION 3.10)
project(MyTest)

enable_testing()

find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

add_executable(MyTest test.cpp)
target_link_libraries(MyTest ${GTEST_LIBRARIES} pthread)

add_test(NAME MyTest COMMAND MyTest)
🧪 ビルドと実行
bash
$ mkdir build && cd build
$ cmake ..
$ make
$ ./MyTest
🔍 4. 主要マクロ一覧
マクロ名	                   説明
TEST(グループ, 名前)	通常のテストケース定義
EXPECT_EQ(a, b)	aとbが等しいことを検証（失敗しても続行）
ASSERT_EQ(a, b)	aとbが等しいことを検証（失敗で即中断）
EXPECT_TRUE(cond)	condがtrueであることを検証
EXPECT_FALSE(cond)	condがfalseであることを検証

🧰 5. テストフィクスチャの使い方
🔹 共通初期化が必要な場合
cpp
class MyTest : public ::testing::Test {
protected:
    void SetUp() override {
        x = 10;
    }

    int x;
};

TEST_F(MyTest, TestXIs10) {
    EXPECT_EQ(x, 10);
}
TEST_F()はテストフィクスチャクラスを使う場合に使用

各テストの前に自動的にSetUp()が呼び出されます

📊 6. テスト結果例
text
[ RUN      ] MyTestSuite.AddFunction
[       OK ] MyTestSuite.AddFunction (0 ms)
[ RUN      ] MyTestSuite.FailingTest
test.cpp:10: Failure
Expected equality of these values:
  add(2, 2)
    Which is: 4
  5
[  FAILED  ] MyTestSuite.FailingTest (0 ms)
[==========] 2 tests ran. 1 passed, 1 failed.
📦 7. インストール方法（Ubuntu例）
bash
sudo apt install libgtest-dev
cd /usr/src/gtest
sudo cmake .
sudo make
sudo cp *.a /usr/lib
またはGitHubから直接ビルド：

bash
git clone https://github.com/google/googletest.git
cd googletest
cmake -Bbuild
cmake --build build
✅ まとめ
TESTでテストを定義

EXPECT_*やASSERT_*で検証

共通初期化にはTEST_FとSetUp()を使用

CMakeと組み合わせて簡単にビルド可能

必要であれば、モックテストのためにgmockも使用可能
