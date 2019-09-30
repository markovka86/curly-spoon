# curly-spoon
This is a fix to libsigrokdecode protocol decoder st7735.
 # Original code
 def put_desc(self, ss, es, cmd, data):
        if cmd == -1:
            return
        if META[cmd]: # This line generates - "ST7735": "Decoder reported an error"
            self.put(ss, es, self.out_ann, [Ann.DESC,
                ['%s: %s' % (META[cmd]['name'].strip(), META[cmd]['desc'])]])
        else:
            # Default description:
            dots = ''
            if len(data) == MAX_DATA_LEN:
                data = data[:-1]
                dots = '...'
            data_str = '(none)'
            if len(data) > 0:
                data_str = ' '.join(['%02X' % b for b in data])
            self.put(ss, es, self.out_ann, [Ann.DESC,
                ['Unknown command: %02X. Data: %s%s' % (cmd, data_str, dots)]])

# Proposed fix
 def put_desc(self, ss, es, cmd, data):
        if cmd == -1:
            return
        if cmd in META:  # This is a fix          
            self.put(ss, es, self.out_ann, [Ann.DESC,
                ['%s: %s' % (META[cmd]['name'].strip(), META[cmd]['desc'])]])
        else:
            # Default description:
            dots = ''
            if len(data) == MAX_DATA_LEN:
                data = data[:-1]
                dots = '...'
            data_str = '(none)'
            if len(data) > 0:
                data_str = ' '.join(['%02X' % b for b in data])
            self.put(ss, es, self.out_ann, [Ann.DESC,
                ['Unknown command: %02X. Data: %s%s' % (cmd, data_str, dots)]])
