# SSC0108 - Prática Sistemas Digitais

[Aula 4 & 5: Contadores.](./lab4.pdf)

### ALUNOS

|        Nome                         |    NUSP   |       
|:-----------------------------------:|:---------:|  
|   Artur Domitti Camargo             |  15441661 |   
|   Lucas Mello Ciosaki       	      |  14591305 |   
|   Lucas Alves da Silva		         |  11819553  | 

### PART I

[RTL VIEWER FOR PART I](./part1b.pdf)

VHDL CODE

```
LIBRARY ieee;
USE ieee.std_logic_1164.ALL;

ENTITY eightbits_synchronous_counter is
	PORT(
		Enable, Clock, clear : in STD_LOGIC;
		 h00, h01, h02, h03, h04, h05, h06, h10, h11, h12, h13, h14, h15, h16: out STD_LOGIC
	);
	
end eightbits_synchronous_counter;

architecture Structural of eightbits_synchronous_counter is
	signal a1, a2, a3, a4, a5, a6, a7, os1, os2, os3, os4, os5, os6, os7, o1, no1, o2, no2, o3, no3, o4, no4, o5, no5, o6, no6, o7, no7, o8, no8 : STD_LOGIC;
	
	component t_flip_flop
		port	(Clk, T, clear: in STD_LOGIC;
				Q, Qn: out STD_LOGIC);
				
	end component;
	
	component seven_segments
		port (A, B, C, D: in STD_LOGIC;
			L0, L1, L2, L3, L4, L5, L6: out STD_LOGIC);
			
	end component;
	
	
	begin
		
		t1: t_flip_flop
			port map (
				T => Enable,
				Clk => Clock,
				clear => clear,
				Q => os1,
				Qn => no1
			);
		
		a1 <= os1 and Enable;
		o1 <= os1;
		
		t2: t_flip_flop
			port map (
				T => a1,
				Clk => Clock,
				clear => clear,
				Q => os2,
				Qn => no2
			);
			
		a2 <= os2 and a1;	
		o2 <= os2;
		
		t3: t_flip_flop
			port map (
				T => a2,
				Clk => Clock,
				clear => clear,
				Q => os3,
				Qn => no3
			);
			
		a3 <= os3 and a2;
		o3 <= os3;
		
		t4: t_flip_flop
			port map (
				T => a3,
				Clk => Clock,
				clear => clear,
				Q => os4,
				Qn => no4
			);
			
		a4 <= os4 and a3;
		o4 <= os4;
		
		t5: t_flip_flop
			port map (
				T => a4,
				Clk => Clock,
				clear => clear,
				Q => os5,
				Qn => no5
			);
			
		a5 <= os5 and a4;
		o5 <= os5;
		
		t6: t_flip_flop
			port map (
				T => a5,
				Clk => Clock,
				clear => clear,
				Q => os6,
				Qn => no6
			);
			
		a6 <= os6 and a5;
		o6 <= os6;
		
		t7: t_flip_flop
			port map (
				T => a6,
				Clk => Clock,
				clear => clear,
				Q => os7,
				Qn => no7
			);
			
		a7 <= os7 and a6;
		o7 <= os7;
		
		t8: t_flip_flop
			port map (
				T => a7,
				Clk => Clock,
				clear => clear,
				Q => o8,
				Qn => no8
			);
			
		hex0: seven_segments
			port map(
				D => o1,
				C => o2,
				B => o3,
				A => o4,
				L0 => h00,
				L1 => h01,
				L2 => h02,
				L3 => h03,
				L4 => h04,
				L5 => h05,
				L6 => h06
				
			);
			
		hex1: seven_segments
			port map(
				D => o5,
				C => o6,
				B => o7,
				A => o8,
				L0 => h10,
				L1 => h11,
				L2 => h12,
				L3 => h13,
				L4 => h14,
				L5 => h15,
				L6 => h16
				
			);
END Structural;
```

T FLIP-FLOP
```
LIBRARY ieee;
USE ieee.std_logic_1164.ALL;

ENTITY t_flip_flop is
	PORT (T, Clk, clear: IN STD_LOGIC;
			Q, Qn: OUT STD_LOGIC);
END t_flip_flop;

architecture Behavioral of t_flip_flop is
signal qb: STD_LOGIC;

begin
	
	
	process(T, Clk)
	begin

		if clear = '1' THEN
			qb <= '0';
			
		ELSIF rising_edge(Clk) THEN
			if T = '1' THEN
				qb <= NOT qb;
			END IF;
		END IF;
		
		Q <= qb;
		Qn <= not qb;
	end process;

end Behavioral;
```

### PART II

[RTL VIEWER FOR PART II](./part2b.pdf)

VHDL CODE 

```
library ieee;
use ieee.std_logic_1164.all;

entity top_level is
    port (
        CLK     : in std_logic;
        ENABLE  : in std_logic;
        CLEAR   : in std_logic;
        HEX0    : out std_logic_vector(6 downto 0);
        HEX1    : out std_logic_vector(6 downto 0);
        HEX2    : out std_logic_vector(6 downto 0);
        HEX3    : out std_logic_vector(6 downto 0)
    );
end top_level;

architecture Behavioral of top_level is
    signal count : std_logic_vector(15 downto 0);
    signal hex_digits : std_logic_vector(15 downto 0);
begin
    counter: entity work.counter16bits
        port map (
            CLK     => CLK,
            ENABLE  => ENABLE,
            CLEAR   => CLEAR,
            COUNT   => count
        );

    D_hex0: entity work.hex_to_7seg
        port map (
            hex_digit => count(3 downto 0),
            seg       => HEX0
        );

    D_hex1: entity work.hex_to_7seg
        port map (
            hex_digit => count(7 downto 4),
            seg       => HEX1
        );

    D_hex2: entity work.hex_to_7seg
        port map (
            hex_digit => count(11 downto 8),
            seg       => HEX2
        );

    D_hex3: entity work.hex_to_7seg
        port map (
            hex_digit => count(15 downto 12),
            seg       => HEX3
        );
end Behavioral;
```

### PART III

[RTL VIEWER FOR PART III](./part3b.pdf)

VHDL CODE
```
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_ARITH.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity flashingNumbers is
port (
			clk	: in STD_LOGIC;
			rst	: in STD_LOGIC;
			seg	: out STD_LOGIC_VECTOR(6 downto 0)
);

end flashingNumbers;

architecture Behavioral of flashingNumbers is
	signal large_counter : unsigned(25 downto 0) := (others => '0');
	signal small_counter : unsigned(3 downto 0) := (others => '0');
	signal enable : STD_LOGIC := '0';
	
	
begin
	process(clk, rst)
		begin
		if rst = '1' then
			large_counter <= (others => '0');
			enable <= '0';
		elsif rising_edge(clk) then
			if large_counter = 49999999 then
				large_counter <= (others => '0');
				enable <= '1';
			else
				large_counter <= large_counter + 1;
				enable <= '0';
			end if;
		end if;
	end process;
	
	process(clk,rst)
		begin
		if rst = '1' then
			small_counter <= (others => '0');
		elsif rising_edge(clk) then
			if enable = '1' then
				if small_counter = 9 then
					small_counter <= (others => '0');
				else
					small_counter <= small_counter + 1;
				end if;
			end if;
		end if;
	end process;
	
	process(small_counter)
	begin
      case small_counter is
            when "0000" => seg <= "1000000";
            when "0001" => seg <= "1111001";  
            when "0010" => seg <= "0100100";
            when "0011" => seg <= "0110000";
            when "0100" => seg <= "0011001";
            when "0101" => seg <= "0010010";
            when "0110" => seg <= "0000010";
            when "0111" => seg <= "1111000";
            when "1000" => seg <= "0000000";
            when "1001" => seg <= "0010000";
            when "1010" => seg <= "0001000";
            when "1011" => seg <= "0000011";
            when "1100" => seg <= "1000110";
            when "1101" => seg <= "0100001";
            when "1110" => seg <= "0000110";
            when "1111" => seg <= "0001110";
            when others => seg <= "1111111"; 
        end case;
    end process;
end Behavioral;
```
