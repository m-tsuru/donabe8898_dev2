+++
title = "かんたんFPGA"
description = "uooo"
date = 2022-09-29
[extra]
[taxonomies]
categories = ["tech"]
tags = ["ArchLinux","FPGA","VHDL"]
+++

# そうだ、FPGAやろう
　我は情報系大学で色々とやっているのですが、論理回路の授業でIntelのQuartus Prime及びQuestaを使って回路を作成しているのですおすし。
本日、授業中にノートPCの電池が無くなってしまい授業続行不可能と判断した結果、**偶然**持ってきてたMac Book Air(ArchLinux入れてるやつ)に
vhdlの環境整えてやりました。

# 行くぜー！1,2,3,4!!!
1.  - **ghdl**と **GTK Wave**をインストール。やり方は各自でググってね♡　僕はArchLinux使いなので以下のコマンドでぶちこんでやるぜ。
        ```bash
        sudo pacman -S ghdl gtkwave
        # ghdlはgcc版を選んだ
        ```
    - Editorは好きなのを使って、どうぞ（VSCodeいいぞ〜これ）。

2.  - テキトーにコードを書く。例として半加算器のコードを載せといてやる夫（＾ω＾　）
        ``` c++
        -- half-adder.vhd
        library IEEE;
        use IEEE.std_logic_1164.all;
        entity HALF_ADDER is
        port (
            A, B: in std_logic;
            S: out std_logic;
            C: out std_logic
        );
        end HALF_ADDER;
        architecture LOGIC of HALF_ADDER is
        begin
            S <= A xor B;
            C <= A and B;
        end LOGIC;
        ```


    - あとテストベンチ用のコードも書いとく
        ```c++
        -- tb_half_adder.vhd
        library IEEE;
        use IEEE.std_logic_1164.all;

        entity TB_HALF_ADDER is
        end TB_HALF_ADDER;

        architecture TB of TB_HALF_ADDER is

        component HALF_ADDER
            port (
                A, B: in std_logic;
                S, C: out std_logic
            );
        end component HALF_ADDER;

        signal A, B, S, C: std_logic;

        begin
            P1: process
            begin
                A <= '0';
                wait for 50 ns;
                A <= '1';
                wait for 50 ns;
            end process;

            P2: process
            begin
                B <= '0';
                wait for 100 ns;
                B <= '1';
                wait for 100 ns;
            end process;

            DUV: HALF_ADDER
            port map (
                    A => A, 
                    B => B, 
                    S => S, 
                    C => C
                );
        end TB;
        ```

3.  - シミュレーションしてみる
        ```bash
        ghdl -a half-adder.vhd      #回路を記述したコードをコンパイル
        ghdl -a tb_half_adder.vhd   #テストベンチもコンパイル
        ghdl -e tb_half_adder       #実行ファイルにする
    
        #シミュレートする。自動では止まってくれないので5,6秒経ったらCtrl+Cで抜けてok。
        ghdl -r tb_half_adder --vcd=half_adder.vcd
        gtkwave half_adder.vcd      #GTKWaveでウェーブを確認できる
        ```

4.  - できあがったものがこちらになります
<img src="gtkwave.png" width="100%">

5.  - 感想
        - Questaいらなくね？