srcs = %w[speed0.m65 chset.m65 speed1.m65 support.m65 dospak.m65 speed2.m65 data.m65]
objs = %w[speedcar.exe speedcar.rom speed.car]

require 'rake/clean'
CLOBBER.include objs

task default: objs

file 'speedcar.exe' => srcs do |t|
  sh 'atasm', t.source, "-o#{t.name}"
end

file 'speedcar.rom' => srcs do |t|
  sh 'atasm', '-r', '-DCARTRIDGE', t.source, "-o#{t.name}"
end

file 'speed.car' => 'speedcar.rom' do |t|
  puts "Creating #{t.name} from #{t.source}"
  File.open(t.name, 'wb') do |f|
    rom = File.binread(t.source)
    cksum = rom.unpack('C*').inject(0, :+)
    f << 'CART'.codepoints.pack('C4')
    f << [1, cksum, 0].pack('L>3')
    f << rom
  end
end
