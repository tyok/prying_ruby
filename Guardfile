# A sample Guardfile
# More info at https://github.com/guard/guard#readme
require "guard/guard"

module ::Guard
  class InlineGuard < ::Guard::Guard

    def start
      UI.info "Starting up server..."
      if running?
        UI.error "Another instance of server is running."
        false
      else
        @pid = fork
        raise "Fork failed." if @pid == -1
        unless @pid
          exec "rake server"
        end
        @pid
      end
    end

    def run_on_change(paths)
      `rake`
    end

    def stop
      UI.info "Shutting down server..."
      Process.kill("TERM", @pid)
      @pid = nil
      true
    end

    def reload
      true
    end

    def running?
      begin
        if @pid
          Process.getpgid @pid
          true
        else
          false
        end
      rescue Errno::ESRCH
        false
      end
    end

  end
end

guard 'inlineGuard' do
  watch /\.scss|\.haml$/
end

# vim: ft=ruby:
