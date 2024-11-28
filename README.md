# SSC0108 - Prática Sistemas Digitais

[Aula 4 & 5: Contadores.](./lab4.pdf)

### ALUNOS

|        Nome                         |    NUSP   |       
|:-----------------------------------:|:---------:|  
|   Artur Domitti Camargo             |  15441661 |   
|   Lucas Mello Ciosaki       	      |  14591305 |   
|   Lucas Alves da Silva		         |  11819553  | 

### PART I e II

VHDL CODE

```
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_ARITH.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

ENTITY top_level IS
    PORT(
         address		: IN STD_LOGIC_VECTOR (4 DOWNTO 0);
			clock		: IN STD_LOGIC  := '1';
			data		: IN STD_LOGIC_VECTOR (3 DOWNTO 0);
			wren		: IN STD_LOGIC ;
			seg   : OUT STD_LOGIC_VECTOR (6 downto 0);
			seg1   : OUT STD_LOGIC_VECTOR (6 downto 0);
			seg2  : OUT STD_LOGIC_VECTOR (6 downto 0);
			seg3   : OUT STD_LOGIC_VECTOR (6 downto 0)
    );
END top_level;

ARCHITECTURE Behavioral OF top_level IS
	signal dataout : std_logic_vector(3 downto 0) := (others => '0');
	signal datain : std_logic_vector(3 downto 0) := (others => '0');
	signal addr_seg1 : std_logic_vector(3 downto 0) := (others => '0');
	signal addr_seg2 : std_logic := '0';
	
	COMPONENT ram32x4
		PORT(
		address		: IN STD_LOGIC_VECTOR (4 DOWNTO 0);
		clock		: IN STD_LOGIC  := '1';
		data		: IN STD_LOGIC_VECTOR (3 DOWNTO 0);
		wren		: IN STD_LOGIC ;
		q		: OUT STD_LOGIC_VECTOR (3 DOWNTO 0)
	);
	END COMPONENT;

BEGIN
	ram : ram32x4
		PORT MAP (
            address => address,           
            clock   => clock,
				wren => wren,
				data => data,
				q => dataout
        );
		  
		  process(dataout, datain, addr_seg1, addr_seg2)
		  begin
			  addr_seg1 <= address(3 downto 0);
			  addr_seg2 <= address(4);
			  datain <= data;
		   case datain is
					when "0000" => seg <= "1000000"; -- 0
					when "0001" => seg <= "1111001"; -- 1
					when "0010" => seg <= "0100100"; -- 2
					when "0011" => seg <= "0110000"; -- 3
					when "0100" => seg <= "0011001"; -- 4
					when "0101" => seg <= "0010010"; -- 5
					when "0110" => seg <= "0000010"; -- 6
					when "0111" => seg <= "1111000"; -- 7
					when "1000" => seg <= "0000000"; -- 8
					when "1001" => seg <= "0010000"; -- 9
					when "1010" => seg <= "0001000"; -- A
					when "1011" => seg <= "0000011"; -- B
					when "1100" => seg <= "1000110"; -- C
					when "1101" => seg <= "0100001"; -- D
					when "1110" => seg <= "0000110"; -- E
					when "1111" => seg <= "0001110"; -- F
					when others => seg <= "1111111"; 
			  end case;
			  
			  case dataout is
					when "0000" => seg1 <= "1000000"; -- 0
					when "0001" => seg1 <= "1111001"; -- 1
					when "0010" => seg1 <= "0100100"; -- 2
					when "0011" => seg1 <= "0110000"; -- 3
					when "0100" => seg1 <= "0011001"; -- 4
					when "0101" => seg1 <= "0010010"; -- 5
					when "0110" => seg1 <= "0000010"; -- 6
					when "0111" => seg1 <= "1111000"; -- 7
					when "1000" => seg1 <= "0000000"; -- 8
					when "1001" => seg1 <= "0010000"; -- 9
					when "1010" => seg1 <= "0001000"; -- A
					when "1011" => seg1 <= "0000011"; -- B
					when "1100" => seg1 <= "1000110"; -- C
					when "1101" => seg1 <= "0100001"; -- D
					when "1110" => seg1 <= "0000110"; -- E
					when "1111" => seg1 <= "0001110"; -- F
					when others => seg1 <= "1111111"; 
			  end case;
			  
			  case addr_seg1 is
					when "0000" => seg2 <= "1000000"; -- 0
					when "0001" => seg2 <= "1111001"; -- 1
					when "0010" => seg2 <= "0100100"; -- 2
					when "0011" => seg2 <= "0110000"; -- 3
					when "0100" => seg2 <= "0011001"; -- 4
					when "0101" => seg2 <= "0010010"; -- 5
					when "0110" => seg2 <= "0000010"; -- 6
					when "0111" => seg2 <= "1111000"; -- 7
					when "1000" => seg2 <= "0000000"; -- 8
					when "1001" => seg2 <= "0010000"; -- 9
					when "1010" => seg2 <= "0001000"; -- A
					when "1011" => seg2 <= "0000011"; -- B
					when "1100" => seg2 <= "1000110"; -- C
					when "1101" => seg2 <= "0100001"; -- D
					when "1110" => seg2 <= "0000110"; -- E
					when "1111" => seg2 <= "0001110"; -- F
					when others => seg2 <= "1111111"; 
			  end case;
			  
			  case addr_seg2 is
					when '0' => seg3 <= "1000000"; -- 0
					when '1' => seg3 <= "1111001"; -- 1
			  end case;
		  end process;
	
END Behavioral;
```

### PART III e IV

VHDL CODE 

```
LIBRARY IEEE;
USE IEEE.STD_LOGIC_1164.ALL;

USE IEEE.NUMERIC_STD.ALL;


Entity memory_array is
	PORT ( 
			address : IN UNSIGNED (4 DOWNTO 0);
			clock : IN STD_LOGIC := '1';
			data : IN STD_LOGIC_VECTOR (3 DOWNTO 0);
			wren : IN STD_LOGIC ;
			q : OUT STD_LOGIC_VECTOR (6 DOWNTO 0);
			ad_out1 : out std_logic_vector(6 downto 0);
			ad_out0 : out std_logic_vector(6 downto 0);
			data_out : out std_logic_vector(6 downto 0)
			
		);
		
END memory_array;

Architecture Behavioral of memory_array is
	TYPE mem IS ARRAY(0 TO 31) OF STD_LOGIC_VECTOR(3 DOWNTO 0);
	SIGNAL memory : mem;
	SIGNAL address_id: INTEGER :=0;
	SIGNAL q_res: std_logic_vector(3 downto 0) := (others => '0');
	SIGNAL ad_p0: UNSIGNED(3 downto 0) := (others => '0');
	SIGNAL ad_p1: std_logic := '0';
	
	begin
	
	
	process(clock, address, data, wren)
	begin
		if(rising_edge(clock)) then
			address_id <= to_integer(unsigned(address));
			if(wren = '1') then
				memory(address_id) <= data;
			end if;
			
			q_res <= memory(address_id);
			
			case q_res is
					when "0000" => q <= "1000000"; -- 0
					when "0001" => q <= "1111001"; -- 1
					when "0010" => q <= "0100100"; -- 2
					when "0011" => q <= "0110000"; -- 3
					when "0100" => q <= "0011001"; -- 4
					when "0101" => q <= "0010010"; -- 5
					when "0110" => q <= "0000010"; -- 6
					when "0111" => q <= "1111000"; -- 7
					when "1000" => q <= "0000000"; -- 8
					when "1001" => q <= "0010000"; -- 9
					when "1010" => q <= "0001000"; -- A
					when "1011" => q <= "0000011"; -- B
					when "1100" => q <= "1000110"; -- C
					when "1101" => q <= "0100001"; -- D
					when "1110" => q <= "0000110"; -- E
					when "1111" => q <= "0001110"; -- F
					when others => q <= "1111111"; 
			  end case;
			  
			  ad_p0 <= address(3 downto 0);
			  ad_p1 <= address(4);
			  
			  
			  case ad_p0 is
					when "0000" => ad_out0 <= "1000000"; -- 0
					when "0001" => ad_out0 <= "1111001"; -- 1
					when "0010" => ad_out0 <= "0100100"; -- 2
					when "0011" => ad_out0 <= "0110000"; -- 3
					when "0100" => ad_out0 <= "0011001"; -- 4
					when "0101" => ad_out0 <= "0010010"; -- 5
					when "0110" => ad_out0 <= "0000010"; -- 6
					when "0111" => ad_out0 <= "1111000"; -- 7
					when "1000" => ad_out0 <= "0000000"; -- 8
					when "1001" => ad_out0 <= "0010000"; -- 9
					when "1010" => ad_out0 <= "0001000"; -- A
					when "1011" => ad_out0 <= "0000011"; -- B
					when "1100" => ad_out0 <= "1000110"; -- C
					when "1101" => ad_out0 <= "0100001"; -- D
					when "1110" => ad_out0 <= "0000110"; -- E
					when "1111" => ad_out0 <= "0001110"; -- F
					when others => ad_out0 <= "1111111"; 
			  end case;
			  
			  case ad_p1 is
					when '0' => ad_out1 <= "1000000"; -- 0
					when '1' => ad_out1 <= "1111001"; -- 1
		
			  end case;
			
			case data is
					when "0000" => data_out <= "1000000"; -- 0
					when "0001" => data_out <= "1111001"; -- 1
					when "0010" => data_out <= "0100100"; -- 2
					when "0011" => data_out <= "0110000"; -- 3
					when "0100" => data_out <= "0011001"; -- 4
					when "0101" => data_out <= "0010010"; -- 5
					when "0110" => data_out <= "0000010"; -- 6
					when "0111" => data_out <= "1111000"; -- 7
					when "1000" => data_out <= "0000000"; -- 8
					when "1001" => data_out <= "0010000"; -- 9
					when "1010" => data_out <= "0001000"; -- A
					when "1011" => data_out <= "0000011"; -- B
					when "1100" => data_out <= "1000110"; -- C
					when "1101" => data_out <= "0100001"; -- D
					when "1110" => data_out <= "0000110"; -- E
					when "1111" => data_out <= "0001110"; -- F
					when others => data_out <= "1111111"; 
			  end case;
			
			
		
		end if;	
	
	end process;
	
end Behavioral;
```
